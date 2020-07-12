# 数据管理

![](../.gitbook/assets/image%20%285%29.png)

在容器中管理数据主要有两种方式：

* 数据卷（Volumes）
* 挂载主机目录 \(Bind mounts\)

## 数据卷

### 数据卷的特性

* `卷` 可以在容器之间共享和重用
* 对 `数据卷` 的修改会立马生效
* 对 `数据卷` 的更新，不会影响镜像
* `数据卷` 默认会一直存在，即使容器被删除

### 创建

```bash
docker volume create my-vol
```

### 查所有数据卷

```bash
$ docker volume ls
```

### 查看详情

```bash
$ docker volume inspect my-vol
```

### 挂载数据卷的方式

#### 方式 1：用镜像启动新容器时候挂载\(两种写法\)

```bash
$ docker run -t -i \
             --mount source=vol-demo,target=/data \
              # -v vol-demo:/data \
             ubuntu
```

#### 方式2: 把容器导出成镜像，然后创建新容器时挂载

#### 方式3：

1. 停止容器
2. commit 该容器
3. 用新生成的镜像重新创建容器

## 挂载主机目录

### 挂载一个主机目录作为数据卷

```bash
$ docker run -d -P \
    --name <container-name> \
    # -v /src/path:/opt/path \
    --mount type=bind, \
    source=/src/<host-path>, \
    target=/opt/<container-path> \
    <image-name> \
    bash
```

Docker 挂载主机目录的默认权限是 `读写`，用户也可以通过增加 `readonly` 指定为 `只读`。

```bash
$ docker run -it \
    --name <container-name> \
    # -v /src/path:/opt/path \
    --mount type=bind,readonly \
    source=/src/<host-path>, \
    target=/opt/<container-path> \
    <image-name> \
    bash
```

### 查看数据卷的具体信息

```bash
$ docker inspect <container-name>
```

### 挂载一个本地主机文件作为数据卷

```bash
$ docker run --rm -it \
   # -v $HOME/.bash_history:/root/.bash_history \
   --mount type=bind,source=$HOME/.bash_history,target=/root/.bash_history \
   ubuntu:18.04 \
   bash

root@2affd44b4667:/# history
1  ls
2  diskutil list
```

