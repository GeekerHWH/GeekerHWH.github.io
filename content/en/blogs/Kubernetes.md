---
title: "Kubernetes for Absolute Beginners"
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

## Concepts
- Node: a physical or virtual machine where k8s installed, and it's where containers will be launched by k8s.
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

## minikube
minikube quickly sets up a local Kubernetes cluster on macOS, Linux, and Windows. it mainly focus on helping application developers and new Kubernetes users.

```bash
# minikube config set driver kvm2
# registry-mirror is an array to store the url of docker registry
minikube start --driver=[docker/kvm2]\
               --registry-mirror=[]

# usable for me
minikube start --driver=docker\
               --image-mirror-country=cn\
               --registry-mirror=https://docker.nju.edu.cn,https://shraym0v.mirror.aliyuncs.com\
               --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers
```

## kubectl
```bash
kubectl run <container> # deploy an application on the cluster
kubectl cluster-info # view the infomation
kubectl get nodes # list all the nodes part of the cluster
kubectl run <pod_name> --image=<image_name>
kubectl delete pods <pod_name>
```

## Deployment Strategies
![](/imagesInBlogs/k8s/deploymentStrategy.png)