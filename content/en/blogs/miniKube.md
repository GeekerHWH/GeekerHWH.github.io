---
title: "MiniKube for Absolute Beginners"
date: 2024-02-21T00:00:00+08:00
draft: false
author: "GeekerHWH"
category: 
tags:
    - containers orchestration
    - Docker
    - Kubernetes
image: /images/blogs/k8s.png
description: ""
toc: true
---

This page is the note for learning Kubernetes, I took this from the Udemy course
[@Mumshad Mannambeth](https://www.udemy.com/course/learn-kubernetes/?couponCode=KEEPLEARNING)

## Theory
### Concepts
- Node: a physical or virtual machine where kubelet was installed, and it's where containers will be launched.
- Cluster: a set of nodes grouped together
- Master: responsible for orchestration and monitor the status of nodes
- [Objects](https://kubernetes.io/docs/concepts/overview/working-with-objects/)
  - `metadata` Data that helps uniquely identify the object, including a name string, UID, and optional lables and namespace
  - `spec` short term for "specification", describes what state you desire for the object

### Components
- API Server: front end to interact with k8s cluster in master
- etcd key store: a distributed reliable key-value store, storing the data used to manage the cluster
- Shedular: responsible for distributing containers across multiple nodes
- Controller: responsible for noticing and responding when nodes, containers, endpoints goes down
- Kublet: the agent that runs on each node in the cluster
- Container Runtime: docker engine
![architecture](/imagesInBlogs/k8s/architecture.png)

## MiniKube Hands-on
MiniKube quickly sets up a local Kubernetes cluster on macOS, Linux, and Windows. it mainly focus on helping application developers and new Kubernetes users.

The cluster is in a VM or a Container, master node and worker node are combined in just one node called "minikube"

Everything we are going to create is within this special node, including Pods, Development, Service, etc...

### Start minikube cluster
```bash
minikube start --driver=docker\
               --static-ip={the_ip_you_want}

# usable for me (as a Chinese user)
minikube start --driver=docker\
               --static-ip=192.168.100.100\
               --image-mirror-country=cn
# or
minikube start --image-repository='registry.cn-hangzhou.aliyuncs.com/google_containers'
```
### Start minikube dashboard
```bash
minikube dashboard
```
then your browser will automatically create a new tab with kubernetes dashboard

![dashboard](/imagesInBlogs/k8s/dashboard.png)


## Deployment
### Deployment Strategies
Recreate vs. RollingUpdate
![](/imagesInBlogs/k8s/deploymentStrategy.png)