# 应用容器化过程

1. 首先获取和编写应用代码
2. 为应用编写Dockerfile，Dockfile 是镜像\(`image`\)的源代码。
3. 通过Dockerfile来构建\(`build`\)  应用的镜像
4. 将镜像交付到物理机和云端仓库（`registry`）（推送和拉取镜像）
5. 最终测试和运行容器化应用（`container`）

![](../.gitbook/assets/image%20%288%29.png)

## 容器的常见状态

* 运行
* 已暂停
* 重新启动
* 已退出

可以通过运行“`docker ps –a`”命令来识别Docker容器的状态

## 容器的生命周期

![](../.gitbook/assets/image%20%282%29.png)

