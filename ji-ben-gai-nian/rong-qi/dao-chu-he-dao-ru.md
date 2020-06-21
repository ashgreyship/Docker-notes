# 导出和导入

## 导出容器/创建快照

如果要导出本地某个容器，可以使用 `docker export <用户名>/<软件名>:<版本号>`

```bash
$ docker container ls -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                    PORTS               NAMES
7691a814370e        ubuntu:18.04        "/bin/bash"         36 hours ago        Exited (0) 21 hours ago                       test
$ docker export 7691a814370e > ubuntu.tar
```

## 导入容器快照

可以使用 `docker import` 从容器快照文件中再导入为镜像

1. 通过本地文件导入：

```bash
$ cat ubuntu.tar | docker import - test/ubuntu:v1.0
$ docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED              VIRTUAL SIZE
test/ubuntu         v1.0                9d37a6082e97        About a minute ago   171.3 MB
```

2. 通过URL导入

```bash
$ docker import http://example.com/exampleimage.tgz example/imagerepo
```

## 导入容器镜像

```text
docker load <image-name>
```

## 导入快照和镜像的区别

* 容器快照文件将丢弃所有的历史记录和元数据信息（即仅保存容器当时的快照状态）
* 镜像存储文件将保存完整记录，体积也要大。此外，从容器快照文件导入时可以重新指定标签等元数据信息。

