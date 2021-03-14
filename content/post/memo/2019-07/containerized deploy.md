---
title: docker, kubernetes, and argo
date: 2019-07-17T01:47:46.713Z
draft: true
---

## docker

相对于传统的直接部署在物理机器上,以及虚拟机(VM)技术,docker 能够:

- 跟具体硬件解耦
- 限制资源获取,更好的利用资源

## kubernetes

用来管理容器.
能够:

- 自动治愈 auto healing
  > 当一个容器挂掉时,自动启动一个新的容器

### basics

`node` 是`kubernetes`管理的 cluster 中的一个节点,可以是一个 VM,或者是物理机器.
`pod` 是`kubernetes` 的一个抽象,在一个 pod 中,可以有多个`container`, 以及
其他的资源如`volumes`等, `pod`运行在 node 中, 一个`node`可以运行多个`pod`

#### 基本命令

- kubernetes get _nodes_ | _pods_ | _services_
- kubernetes describe _nodes_ | _pods_
- kubernetes logs
- kubernetes proxy
  > 运行一个 proxyserver, 可以通过 api 来与 container 通信
-

`kubernetes`

### deployment

### service

### scaling

> Scaling is accomplished by changing the number of replicas in a Deployment.

扩展性是通过在一个`deployment`中修改`replicas`的数量来实现的.

### rolling update

`kubernetes` allow to update your app without any downtime, through incrementlly add new replica to replace old app.

## argo

**argo** 将工作流*workflow*引入了**kubernetes**, 通过配置文件,
argo 能够将一系列的运行步骤发送给 kubernetes,实现灵活的 app 运行.
类似于 ci 中*workflow*的概念, argo 通过`input`, `output`, `steps`,以及
`dependencies` 等方式, 配置 app 的运行.

### 实现方式

TODO:

## tools

- helm
  > kubernetes 的包管理工具

* minikube

  > 为本地开发 cluster 的 kubernetes 工具, 只维护一个 node

* kubectl

  > kubernetes 的命令行工具

* argo
  > argo 项目的命令行工具
