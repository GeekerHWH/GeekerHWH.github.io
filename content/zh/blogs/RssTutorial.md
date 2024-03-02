---
title: "如何订阅博客，摆脱推荐算法"
date: 2024-02-02T00:00:00+08:00
draft: false
author: "GeekerHWH"
zh_tags: 
    - RSS
    - Blogs
image: /images/blogs/RSS-feed.jpg
description: ""
toc: true
---

## 前言
在这个大数据形成信息茧房绑架用户的时代，有没有一种玩意可以实现订阅自由高效上网呢？
正好最近我开始搭建个人博客，希望保留住不小心打开本站的用户，那就给大家分享分享RSS这个古玩吧。

## 什么是RSS
RSS（Really Simple Syndication）是一种用于获取网站更新的标准化方式。它允许用户通过RSS阅读器订阅喜欢的网站，实时获取最新文章、新闻或内容更新的摘要和链接，而无需浏览整个网站。RSS提供了一种简便的方式，使用户能够集中阅读多个来源的信息，节省时间和努力。网站所有者可以通过生成并发布RSS提要，将其网站内容推送给感兴趣的用户。这种格式化的数据传输方式促进了信息的广泛传播和分享，使用户能够更轻松地跟踪他们关注的主题和内容。

想必大家总是可以在独立博客网站看到一个类似Wi-Fi的图标，点进去跳转到一堆字符的界面，这其实就是RSS订阅链接啦，
比如本页最下方或本站主页都有这样的订阅链接。一般博客网站的站主都提供这样的链接以便读者订阅他们精心制作的网站。
那么如何才能通过该链接进行订阅呢？

## Fluent Reader (Lite)
我推荐一个我自己用的RSS阅读器吧，Fluent Reader。

![Fluent Reader](/imagesInBlogs/RSS/fluentreader.png)

该软件优点：
- 支持仅RSS文本阅读、全内容阅读、内部加载全网页阅读、外部浏览器打开阅读四种模式，总有一款适合你
- 支持设置PAC代理文，订阅大陆境外博客
- 支持Windows / Linux / MacOS / Android / iOS
- 交互设计简洁大方
- 支持分组订阅以及基于规则的Filter
- ......

### 下载
桌面端软件可以通过微软商店/Mac软件商店下载，如果你是Linux玩家直接从Github下载Appimage即可

移动端如果你是iOS不会折腾TestFlight的话就必须得花1.99刀支持一下作者啦，如果你是安卓则可以前往
[Github: Fluent Reader Lite](https://github.com/yang991178/fluent-reader-lite/releases)
下载apk进行侧载，当然也可以去Google play支持一下作者。

### 使用
以我的中文博客举例，这是我的中文区的RSS订阅链接
https://geekerhwh.github.io/zh/blogs/index.xml
将其复制，打开Fluent Reader点击右上角的设置按钮将该链接复制到add source中即可，
点击添加然后就会看到你所订阅网站的更新内容啦！是不是感觉B站知乎小红书从此可以让位“互联网入口”之争了。

![example](/imagesInBlogs/RSS/example.png)

大家赶紧用我的订阅链接尝试起来，关爱一下空巢站主吧TvT

### 更多
更多高级的使用功能以后再更新，写博客就是突出一个任性，反正没多少人看