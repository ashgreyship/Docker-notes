# Q&A

## 什么是docker swarm？

**docker-swarm是解决多主机多个容器调度部署得问题。**

Docker Swarm是Docker原生的集群管理和调度工具，它将Docker主机池转变为单个虚拟Docker主机。Docker swarm提供标准的Docker API，任何已经与Docker守护进程通信的工具都可以使用Swarm透明地扩展到多个主机。

## 什么是Docker Compose**？**

**dcoker-compose主要是解决本地docker容器编排问题。**

Docker Compose是一个定义和运行多容器应用的工具，通过编写Compose文件来定义和配置应用，只需要用docker-compose一条命令就可以一次把所有定义好的服务启动起来。

## **Docker如何在非Linux系统中运行容器？**

通过添加到Linux内核版本2.6.24的名称空间功能，可以实现容器的概念。容器将其ID添加到每个进程，并向每个系统调用添加新的访问控制检查。它由clone（）系统调用访问，该调用允许创建先前全局命名空间的单独实例。

如果由于Linux内核中可用的功能而可以使用容器，那么显而易见的问题是非Linux系统如何运行容器。Docker for Mac和Windows都使用Linux VM来运行容器。Docker Toolbox用于在Virtual Box VM中运行容器。但是，最新的Docker在Windows中使用Hyper-V，在Mac中使用Hypervisor.framework。

## Docker Engine

Docker Engine是用于运行和编排容器的基础设施工具，我们平时说到的Docker大多数指的是Docker Engine，也就是在命令行和Docker进行交互的时候打交道的后台进程。

这是Docker Engine目前的架构，Docker客户端通过REST API与Docker Daemon来进行交互，Daemon把命令下发给containerd，containerd负责容器的生命周期管理以及镜像管理等，而runc负责创建容器。

Docker首次发布时，Docker Engine由两个核心组件构成：LXC和Docker Daemon。Docker Daemon是单一的二进制文件，包含诸如 Docker客户端、Docker API、容器运行时、镜像构建等。LXC提供了对Namespace\(资源隔离\)和CGroup\(资源限制\)等基础工具的操作能力，它们是基于Linux内核的容器虚拟化技术。



