---
title: "如何读源码？"
date: 2024-04-22T00:00:00+08:00
draft: true
author: "GeekerHWH"
zh_tags:
    - Open Source
    - Github
image: /images/blogs/readSourceCode.jpeg
description: "How to read source code"
toc: true
---

本文将以github.com/xtls/xray-core和Nekoray为例，说明我们该如何阅读源代码

提前声明本文仅科普技术，不鼓励不教唆翻墙，不涉及政治口水

在阅读源码之前你可以做以下事情：
- 阅读完README.md文件
- 确认是否有官方架构文档或设计文档
- 从运维层面了解该项目的各种功能

# 查看项目仓库，了解文件夹结构
这一步的关键是弄明白项目中每个文件夹是用来实现什么功能的

下面我们来看看xray-core的结构

![xray-core项目仓库结构](/imagesInBlogs/readSourceCode/xray-core.png)

.github/一般而言是用于github内嵌功能相关的文件，比如Github Actions定义文件等

至于其他部分就需要我们结合文档来进行分析了，这里我们可以打开ProjectX的官方文档[Project X](https://xtls.github.io/)，可以看到Project X的开发指南是非常详细的，我们需要的是查看 开发指南->设计目标->架构
![xray-core设计架构](/imagesInBlogs/readSourceCode/xrayArchitecture.png)

可以看见xray总共分三个逻辑层：“应用层、代理层和传输层。
每一层内包含数个模块，模块间互相独立，同类型的模块可无缝替换......“（摘自原文档）

那么结合上面我们看到的xray-core项目仓库结构我们可以猜测几个文件夹的作用了：
- app/ 这个文件夹大概率实现了xray应用层需要的功能
- proxy/ 这个文件夹大概率实现了代理层需要的各种加密通讯协议
- transport/ 这个文件夹大概率**实现或封装**了最基础的传输层协议

我们可以看看proxy/以验证我们的猜想：

![proxy文件夹](/imagesInBlogs/readSourceCode/proxy.png)

看来我们的猜测十分正确！

当然啦，不是所有的项目都有如此规范的开发文档供我们参考，针对开发者的文档我们还有一个十分重要的传统，那就是放在docs/文件夹内。本处xray是没有的，我们看看xray神一样的工具们Nekoray的docs/里都有些什么

![nekorayDocs文件夹](/imagesInBlogs/readSourceCode/nekodocs.png)

看到了吗，几乎完全是针对开发者所编撰的，比如各个平台如何编译nekoray_core的文档。经常有伸手党完全不体会开发者工作生活的繁忙，要求GUI作者时刻警惕上游核心更新，其实我们大可自行尝试跟随文档自行编译。

# 查看编译文件，了解模块依赖
