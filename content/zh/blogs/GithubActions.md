---
title: "Github Actions CI/CD自动化工作流详解"
date: 2024-02-20T00:00:00+08:00
draft: false
author: "GeekerHWH"
zh_tags:
    - Github Action
    - Development
image: /images/blogs/GithubActions.jpg
description: "Github Actions详解"
toc: true
---

## 概览
Github Actions是一个持续集成(Continuous Integration)和持续部署(Continuous Delivery/Deployment)工具平台，你可以用它打造编译、测试、部署、发布一条龙自动化流水线。Github Actions与Github生态深度绑定，你可以用Actions来测试PR的代码或者发布Release等常规操作，所有这些只需要你定义好触发条件、反应行为即可，而定义这些的代码及其简单，只要你的英语能过高考，就可以当白话文一样看懂。另外，本博客其实就是用Github Actions自动部署生成到Github Pages的哦，如果你有兴趣的话可以到本博客的开源仓库看看Github Actions的配置文件的啦。[仓库地址](https://github.com/GeekerHWH/GeekerHWH.github.io)

### 示例
不妨来看一下我项目mirrorSpeedTest中的一个将Go程序编译并测试的工作流(Workflow)配置文件吧:
```yml
name: Go

on:
  push:
    branches: [ "main" ]

jobs:

  build and test basic function:
    runs-on: ubuntu-22.04
    steps:
    - uses: actions/checkout@v3

    - name: Set up Go
      uses: actions/setup-go@v4
      with:
        go-version: '1.21'

    - name: Build
      run: go build -o mirrorSpeedTest main/main.go

    - name: Test cmd mode for US
      run: ./mirrorSpeedTest -c US
```
我们首先给工作流起名字叫Go，这条工作流在什么时候出发我们用`on`关键词定义，即在我们将代码push到main这条分支的时候会触发该文件定义的Github Actions。那么触发后它要自动帮我做什么呢，我们在`jobs`中定义我们需要的工作，`jobs`是一个数组，定义着一系列有序的工作，本例中就只有build and test basic function这一个工作了。如何描述该工作呢，我们需要描述我们需要的编译平台，这里是ubuntu 22.04操作系统。我们有一系列的步骤，这里用到了github官方做好的两个步骤即actions/checkout@v3，帮助我们将代码从仓库拉到运行actions的这台服务器上，actions/setup-go@v4帮我们为这台服务器设置go的编译环境，可以看到这还能指定go的版本。如果你曾经自己设置过go或者C或者Python，你知道其实这一大步可以分为很多小步，即便是用脚本那也要curl下载官方文件、tar解压、配置环境变量等等......而这一切Github Actions都帮你简化了，打包了，你只需要在[Github Marketplac](https://github.com/marketplace)中找到你所需要的动作模块就行啦！
当然，如果没有满足我们要求的命令我们也可以用`run`关键字执行标准的Linux命令或者Windows命令来解决，比如本例中我要编译我自己刚开发的程序，那我只是一个平平无奇的蜗居在互联网角落里写博客的小人物，Github又怎么会专门给我的mirrorSpeedTest写一个Actions呢，这时候就可以自行写编译命令以及功能测试命令了。