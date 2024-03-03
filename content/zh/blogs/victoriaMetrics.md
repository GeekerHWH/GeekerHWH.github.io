---
title: "Victoria Metrics(VM)概念入门"
date: 2024-05-20T00:00:00+08:00
draft: false
author: "GeekerHWH"
zh_tags:
    - CloudNative
    - Database
image: /images/blogs/victoriaMetrics.png
description: 
toc: true
---

> VM有单实例版本和集群版本，本篇内容仅讨论集群版本

### 引入问题：VM vs Prometheus
VictoriaMetrics是一个高性能、可伸缩的时序数据库，它旨在解决Prometheus在处理大规模监控数据时遇到的一些问题。具体来说，VictoriaMetrics针对以下几个方面提供了改进：
1. 存储效率：VictoriaMetrics使用高效的压缩算法来存储时间序列数据，相比Prometheus，它能以更低的存储成本来保存同样量级的数据。
2. 查询性能：VictoriaMetrics针对大数据量的查询进行了优化，能够更快地返回查询结果，尤其是在处理复杂查询时表现更为出色。
3. 水平扩展能力：虽然Prometheus也支持通过Federation等方式进行扩展，但VictoriaMetrics设计之初就考虑到了水平扩展的需求，提供了更为简便和高效的扩展方案，支持无缝地增加节点（例如vmselect和vminsert）来处理更多的数据。
4. 长期存储：Prometheus的长期存储方案通常需要额外的组件（如Thanos或Cortex）来实现，而VictoriaMetrics本身就提供了高效的长期存储能力，简化了架构。
5. 5.高可用性：VictoriaMetrics提供了内置的高可用性支持，简化了部署和管理高可用监控系统的复杂度，官方提供原生的Operator支持VictoriaMetrics Operator。
6. 易用性兼容性：VictoriaMetrics支持PromQL，这意味着原有的Prometheus查询可以直接在VictoriaMetrics上运行，降低了迁移成本，自然兼容Grafana监控面板 。

### VM集群版本架构设计
![VM_Architecture](/imagesInBlogs/VictoriaMetrics/arch.png)
分为前端客户端、无状态查询(Read)计算层、Stateful时序数据存储层、无状态插入(Write)计算层、后端数据收集层
这里涉及三个关键组件：
- vmselect：根据查询要求从vmstorage中读取合并数据
- vminsert：将摄取的数据根据  指标名称与所有标签计算出的一致性哈希值  在 vmstorage 节点之间散播数据
- vmstorage：持久化存储原始数据并返回  给定标签在给定时间范围内的查询数据 

### VM-Operator的CRD(Customized Resource Definetion)概念
- VMCluster：VMCluster 代表了 VictoriaMetrics 数据库的高可用和高容错版本，是Operator关照的对象，对于每个VMC，operator会依次创建下列对象并保证其状态 
  - VMStorage (Statefulset) 和对应的headless service
  - VMSelect (Statefulset) 和对应的headless service
  - VMIsert (Deployment) 和对应的ClusterIP
  - VMAgent (Statefulset) VMAgent 是一个轻量级代理，可从各种来源（exporter/agent）以不同方式收集（通过Pull/Push）指标(Metrics)、重新标签以及过滤收集的指标(Metrics)，并通过 Prometheus remote_write 协议或 VictoriaMetrics remote_write 协议将它们存储在 VictoriaMetrics 或任何其他存储系统中。

![VMAgent](/imagesInBlogs/VictoriaMetrics/vmagent.png)