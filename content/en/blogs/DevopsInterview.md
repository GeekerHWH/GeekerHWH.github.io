---
title: "Devops Interview QA Collection"
date: 2024-02-29T08:55:56+08:00
draft: false
author: "GeekerHWH"
category: 
tags: 
    - DevOps
image: /images/blogs/Devops.jpeg
description: ""
toc: true
---

## Database & SQL
1. Mysql master-slave replication
    - Advantages:
      - if something wrong with master, the request can be as quickly as possible redirected to slave.
      - divide Reads and Writes, slaves can take on some read requests, which depressures the master.
      - asynchronous backup, which keeps master running.
    - Principle: the binlog (binary log) will record every change that was made in master, the ioThread in slave will read that binlog and write the changes in "Relay log". After that, the "SQLThread" will perform the changes to the slave's data.
![master-slave](/imagesInBlogs/DevopsInterview/master-slave.png)

## Operating System
1. RAID

RAID 0: 2 x Blocks to get 2 times faster IO speed, with no redundancy

![RAID 0](/imagesInBlogs/DevopsInterview/RAID0.jpg)

RAID 1: 2 x Blocks to get 1 redundant copy, with no IO speed up

![RAID 1](/imagesInBlogs/DevopsInterview/RAID1.jpg)

RAID 5: 3 x Blocks to get 1 redundant copy, with theoriodically 3X speed up

![RAID 5](/imagesInBlogs/DevopsInterview/RAID5.jpg)

RAID 10: RAID 1 x RAID 0

![RAID 10](/imagesInBlogs/DevopsInterview/RAID10.jpg)
## Networking