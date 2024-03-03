---
title: "Podman教程"
date: 2024-03-02T20:03:04+08:00
draft: true
author: "GeekerHWH"
category: 
tags: 
    - Podman
    - container
    - virtualization
image: 
description: ""
toc: true
---

## Podman是什么
Podman是一个无守护进程、开源的Linux原生工具，它使得创建运行部署容器以及容器镜像更加简单。
所有习惯于使用Docker的用户都可以迅速上手Podman，你甚至可以直接给podman创建一个别名叫做docker都没有任何问题：`alias docker='podman'`。虽然说Podman是一个无守护进程的，它仍然依赖于一个Container Runtime来与操作系统进行交互来创建容器。Podman既可以使用root身份运行也可以使用非特权用户运行，它调用 libpod 来管理整个容器生态。Podman的容器可以使用RESTFul API来进行管理。



## 安装
基于Debian的操作系统
```bash
sudo apt install podman -y
```


