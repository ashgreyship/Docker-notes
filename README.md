# 介绍

## 声明

此为Docker 从入门到实践读书笔记

## 容器介绍

### 什么是Docker

Docker是一个借助容器进行开发，部署和运行应用的平台。

基于 Go 语言开发。基于Linux 的Cgroup , namespace 以及 [OverlayFS](https://docs.docker.com/storage/storagedriver/overlayfs-driver/) 类的 [Union FS](https://en.wikipedia.org/wiki/Union_mount) 等技术。

* **Cgroup** : Cgroups 是 control groups 的缩写，是 Linux 内核提供的一种可以限制、记录、隔离进程组（process groups）所使用的物理资源（如：cpu,memory,IO等等）的机制。
* **namespace**:  ****Linux 内核用来隔离内核资源的方式**。**通过 namespace 可以让一些进程只能看到与自己相关的一部分资源，而另外一些进程也只能看到与它们自己相关的资源，这两拨进程根本就感觉不到对方的存在。
* **OverlayFS**: 一种堆叠文件系统，它依赖并建立在其它的文件系统之上.
* **Union FS**: 多个目录_mount_到同一个挂载点

## 容器和虚拟机的区别

![&#x865A;&#x62DF;&#x673A;&#x4E0E;&#x5BB9;&#x5668;](.gitbook/assets/image%20%284%29.png)

### 虚拟机: 

每个虚拟机都有自己的**完整操作系统**，因此在运行内置于虚拟机的应用程序时，内存使用量可能会高于必要值，虚拟机可能会开始耗尽主机所需的资源

### 容器

与传统的容器化应用程序不同，**共享操作系统环境**（内核），因此它们比完整虚拟机使用更少的资源，并减轻主机内存的压力。

## Docker 的特点

### 更高效的利用系统资源

不需要进行硬件虚拟以及运行完整操作系统

### 更加快速的启动环境

由于直接运行于宿主内核，无需启动完整的操作系统

### 一致的运行环境

Docker 的镜像提供了除内核外完整的运行时环境，确保了应用运行环境一致性

### 持续交付和部署

开发人员可以通过 Dockerfile 来进行镜像构建，并结合 [持续集成\(Continuous Integration\)](https://en.wikipedia.org/wiki/Continuous_integration) 系统进行集成测试，而运维人员则可以直接在生产环境中快速部署该镜像，甚至结合 [持续部署\(Continuous Delivery/Deployment\)](https://en.wikipedia.org/wiki/Continuous_delivery) 系统进行自动部署。

### 更轻松的迁移

确保了执行环境的一致性，使得应用的迁移更加容易

### 更轻松的维护和扩展

Docker 使用的分层存储以及镜像的技术，使得应用重复部分的复用更为容易，也使得应用的维护更新更加简单

## 实现隔离机制

* Linux 命名空间：使每个进程只看到自己的系统视图（文件系统，用户ID,网络接口）
* Linux 控制组（cgroups）:限制了进程能使用的资源量（CPU，内存，网络带宽）

