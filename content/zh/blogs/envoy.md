---
title: "Envoy学习笔记"
date: 2024-06-10T00:00:00+08:00
draft: false
author: "GeekerHWH"
zh_tags:
    - CloudNative
    - Proxy
    - Service Mesh
    - Load Balancer
image: /images/blogs/envoy.jpg
description: "What, Why, How"
toc: true
---

# What is Envoy
Envoy是为超大型现代面向服务架构设计的通讯总线应用，项目秉承着这样的哲学：
> 网络应该对应用层是不可见的。当问题出现的时候理应很容易发现问题的根源

在实践中，Envoy通过以下手段达成其哲学目标：
- **边车架构**：Envoy自己单独打包为一个容器进程在每一台应用服务器上运行，所有的Envoys组成了一个透明通讯网络，应用程序收发消息但是意识不到这样一个网络的存在。这样的架构相比于传统基于编程语言使用网络库的方式有如下两个好处：
  - Envoy可以和所有语言进行交互，一个Envoy网络可以支撑庞大的service oriented架构以及多种应用框架
  - 可以快速方便的透明的更新整个网络设施
- **网络传输层filter架构**：Envoy的核心是一个网络层传输层代理。可插入的filter链机制允许编写不同的filter来执行不同的代理任务，Filter可以支持不同的任务
比如TCP代理、UDP代理、HTTP代理、TLS客户端证书验证、Redis, MongoDB, Postges......
- **HTTP应用层filter架构**：HTTP filter可以作为插件加入HTTP连接管理子系统以支持比如说buffering、ratte limiting、routing/forwarding、sniffing等任务

## Envoy术语规范
- **Host**：指可以单独被寻址的网络通讯主体，一个物理机上可能有很多个hosts，只要可以被独立寻址
- **Downstream**：dowstream host连接到Envoy发送请求、接受应答，其实也就是我们常说的”前端“，Anything in front of Envoy is Downstream
- **Upstream**：upstream host从Envoy接收连接和请求并响应回复应答，其实也就是我们常说的”后端“，Anything in back of Envoy is Upstream
> 前后端是个相对的概念，尤其是用Envoy做Service Mesh时候，两个服务互相通讯，前端和后端往往没有那么容易区分也没有区分的必要
- **Listener**：Envoy的一个模块，负责绑定到 IP/Port，接受新的 TCP 连接（或 UDP 数据报），它可以被downstream client连接。Envoy会暴露一个或多个listeners供downstream连接
- **Cluster**：一个cluster是一组逻辑上Envoy连接的upstream hosts，Envoy通过service discovery发现cluster中的成员。它可选通过active health checking决定成员健康。请求成员的路由会被load balancing policy决定，理论上一个Cluster对应一种服务
- **Mesh**：一组可以协作提供持久网络拓扑结构的hosts。本文中“Envoy Mesh”就是一组Envoy proxies组成的包含不同服务以及应用程序的分布式系统消息传递平台
- **Filter**：连接或请求处理pipline中的模块，提供请求处理的某些功能。
- **Fileter chain**：一系列Filters
- **Runtime configuration**：实时配置系统随Envoy一起部署，配置设置可以无需重新启动Envoy或修改原配置就可以生效

## 网络拓扑结构
一个请求如何流经网络中的组件（包括 Envoy）取决于网络的拓扑结构。Envoy 可用于多种网络拓扑结构。本节将简要介绍 Envoy 与网络其余部分的关系。

Envoy最初是作为Sidecar代理形式存在的，它从应用的Service中分离出load balancing, routing, observability, security和discovery services。在Service Mesh模型中，请求将 Envoy作为本地网关流向网络。请求通过ingress listeners 或egress listeners到达Envoy：
- 入栈Ingress Listeners从Service Mesh的其他节点接收到请求并把它们转发到本地应用程序Upstream，本地应用程序的应答返回给Envoy再发送给Downstream。
- 出栈Egress Listeners从本地应用程序接收请求并将它们转发给网络中的其他节点，其他这些节点一般来说也有Envoy边车，并且通过Ingress Listeners接收这些被转发的请求。

![envoy](/imagesInBlogs/envoy/envoy-network-topology-service-mesh.svg)

Envoy可以进行多种多样的配置，除了在Sevice Mesh的场景下，还可以用于内部负载均衡器（交换机）
![envoy](/imagesInBlogs/envoy/envoy-network-topology-internal-loadbalancer.svg)

Envoy也可以在网络边缘用于ingress/egress proxy
![envoy](/imagesInBlogs/envoy/envoy-network-topology-proxy.svg)

当然，实际应用场景中上述情况都是混用的，一些Envoy用于Service Mesh，一些应用于内部负载均衡器，一些用于出入栈网关，一条请求可能流经多个Envoys
![envoy](/imagesInBlogs/envoy/envoy-network-topology-hybrid.svg)

如果需要拓展性Scalability和可靠性Reliability，Envoy可以被配置为多层网络拓扑
![envoy](/imagesInBlogs/envoy/envoy-network-topology-multi-tier.svg)

总的来说，一个请求会从Downstream通过TCP / UDP / Unix domain sockets到达Envoy，Envoy会转发这些请求给Upstream。接下来我们关心单个的Envoy proxy。

## High level architecture
Envoy中请求处理的路径分为两个部分：
- Listener subsystem：处理下游的请求，并负责管理下游请求和应答的生命周期
- Cluster subsystem：负责选择和配置upstream连接到一个提供服务的端点，这里也是处理cluster成员健康探活、负载均衡、连接池管理的地方，upstream HTTP/2编解码器也在这一部分

这两个子系统通过HTTP router filter进行桥接，该filter将请求从downstream转发到upstream

![envoy](/imagesInBlogs/envoy/envoy-high-level-architecture.svg)

上面两个子系统用于指代由ListenerManager和ClusterManager类创建的一系列模块和实例，我们之后讨论的很多组件都是在这些管理系统发出请求之前和请求过程中被实例化的，比如Listeners, Filter chains, codecs, connection pools以及load balancing data structures

Envoy是envent-based thread模型，并且是单进程多线程的架构。一个主线程负责服务端生命周期、配置管理、统计等任务，一堆worker threads负责处理请求执行监听、过滤、转发，总的来说，Envoy是100%非阻塞的。所有的线程都围绕着event loop([libevent](https://libevent.org/))进行操作，任何给定的downstream TCP连接（包括所有多路复用流）的生命周期都将被唯一一个worker thread处理。每一个worker线程都维护着自己的对upstream端点的TCP连接池。对于UDP的处理则使用SO_REUSEPORT使内核通过持续的hash source/destination IP:port将消息给同一个worker线程

由于Worker threads很少共享状态并且以普通的并行方式进行操作，这种线程模型可以扩展到非常高的CPU核心数

### Listener连接负载均衡
默认情况下，worker线程间没有协作，这意味着所有的worker线程都独立的尝试从每一个listener上接收连接，并基于操作系统内核来执行不同线程间的负载均衡

对于大多数工作流，内核都能做得非常不错。但是对于一小部分超级长的连接比如说HTTP2/gRPC的情况，最好还是Envoy介入，通过不同的connection balancing来配置每一个listener

## Deployment types
### Service to Service only
![service-to-service](/imagesInBlogs/envoy/service-to-service.svg)
上图展示了最简单的Envoy部署方式，即将Envoy作为Service Oriented Architecture (SOA)架构内部的通讯总线。在这种情形下，Envoy只需要启动几个给本地流量和service-to-service的流量使用的listener即可。

#### Service to Service Egress Listener
![service-to-service](/imagesInBlogs/envoy/service-to-service-egress-listener.svg)

#### Service to Service Ingress Listener
![service-to-service](/imagesInBlogs/envoy/service-to-service-ingress-listener.svg)

### Service to Service plus front proxy
![service-to-service](/imagesInBlogs/envoy/service-to-service-front-proxy.svg)
上图展示了一个位于HTTP L7边界反向代理后方的service to service的配置，该反向代理提供如下功能：
- 中止TLS减少内部算力开销
- 支持HTTP/1.1, HTTP/2, HTTP/3
- 全功能HTTP层级路由
- 外界通过ingress port和Service to Service Envoy Cluster交互，并使用服务发现service来查询主机。因此，前置的Envoy工作方式其实与其他任何Envoy相同，只是它不与其他服务并置运行。

## ToDo：Configuration

# 优秀资源推荐
- [Envoy Proxy Crash Course, Architecture, L7 & L4 Proxying, HTTP/2, Enabling TLS 1.2/1.3 and more](https://www.youtube.com/watch?v=40gKzHQWgP0)