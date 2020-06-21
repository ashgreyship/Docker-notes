# Docker 基本架构

Docker 主要有以下几部分组成：

1.  Docker Client 客户端
2.  Docker daemon 守护进程
3.  Docker Image 镜像
4.  Docker Container 容器
5.  Docker Registry 仓库

客户端和守护进程：

*  Docker是C/S（客户端client-服务器server）架构模式。
*  [docker](https://youzhixueyuan.com/tag/docker)通过客户端连接守护进程，通过命令向守护进程发出请求，守护进程通过一系列的操作返回结果。
*  [docker](https://youzhixueyuan.com/tag/docker)客户端可以连接本地或者远程的守护进程。
*  [docker](https://youzhixueyuan.com/tag/docker)客户端和服务器通过socket或RESTful API进行通信。

![Docker architecture](../.gitbook/assets/image%20%2810%29.png)

