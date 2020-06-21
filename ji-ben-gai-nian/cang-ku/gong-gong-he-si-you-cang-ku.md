# 公共和私有仓库

## 公共仓库：

Docker hub是一个基于云的注册表服务.允许您链接到代码存储库，构建镜像并测试它们，存储手动推送的镜像以及指向Docker云的链接，以便您可以将镜像部署到主机。

`docker login` 和 `docker logout` 用于登录公共仓库的操作。

## 私有仓库:

[`docker-registry`](https://docs.docker.com/registry/) 是官方提供的工具，可以用于构建私有的镜像仓库.

### 运行官方 `registry` 镜像：

```bash
$ docker run -d -p 5000:5000 --restart=always --name registry registry
```

默认情况下，仓库会被创建在容器的 `/var/lib/registry` 目录下。

## 在私有仓库上传、搜索、下载镜像

先在本机查看已有的镜像

```bash
$ docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
ubuntu                            latest              ba5877dc9bec        6 weeks ago         192.7 MB
```

使用 `docker tag` 将 `ubuntu:latest` 这个镜像标记为 `127.0.0.1:5000/ubuntu:latest`。

```bash
$ docker tag ubuntu:latest 127.0.0.1:5000/ubuntu:latest
```

### 上传镜像

```bash
$ docker push 127.0.0.1:5000/ubuntu:latest
```

### 查看仓库中的镜像

```bash
$ curl 127.0.0.1:5000/v2/_catalog
```

### 下载镜像

```bash
$ docker pull 127.0.0.1:5000/<repo-name:tag>
```

