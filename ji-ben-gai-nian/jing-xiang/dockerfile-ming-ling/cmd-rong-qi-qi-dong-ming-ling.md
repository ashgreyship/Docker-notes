# CMD & ENTRYPOINT

## **CMD**

`CMD` 是容器启动命令

两种格式：

* `shell` 格式：`CMD <命令>`
* `exec` 格式：`CMD ["可执行文件", "参数1", "参数2"...]`
* 参数列表格式：`CMD ["参数1", "参数2"...]`。在指定了 `ENTRYPOINT` 指令后，用 `CMD` 指定具体的参数。

一般推荐使用 `exec` 格式，这类格式在解析时会被解析为 JSON 数组，因此一定要使用双引号 `"`，而不要使用单引号。

* 但是 `exec` 模式的特点是不会通过shell执行相关命令：

```text
FROM ubuntu
CMD [ "echo", "$HOME" ]
```

运行此镜像, $home这样的环境变量是无法取值的。

* 通过 exec 模式执行 shell 可以获得环境变量：

```text
FROM ubuntu
CMD [ "sh", "-c", "echo $HOME" ]
```

以上镜像被运行就可以得到$HOME 的值。

{% hint style="warning" %}
Docker 不是虚拟机，容器中的应用都应该以前台执行，而不是像虚拟机、物理机里面那样，用 `systemd` 去启动后台服务，容器内没有后台服务的概念
{% endhint %}

#### 执行以下命令后立即退出：

```text
CMD service nginx start
```

`CMD service nginx start` 会被理解为 `CMD [ "sh", "-c", "service nginx start"]`，因此主进程实际上是 `sh`。那么当 `service nginx start` 命令结束后，`sh` 也就结束了，`sh` 作为主进程退出了，自然就会令容器退出。

正确的做法是直接执行 `nginx` 可执行文件，并且要求以前台形式运行。比如：

```text
CMD ["nginx", "-g", "daemon off;"]
```

## ENTRYPOINT

`ENTRYPOINT` 的目的和 `CMD` 一样，都是在指定容器启动程序及参数。

也是支持 `shell` 和 `exec` 两种格式。

### 使用场景

#### 场景一：让镜像变成像命令一样使用

如果使用 `CMD`

```text
FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
CMD [ "curl", "-s", "https://ip.cn" ]
```

使用 `CMD` 时候运行镜像:

```bash
docker run image-name -i
```

运行失败，因为镜像名字后面只能跟命令，不能跟参数。这里 `-i` 会替换成 dockerfile里面 CMD的命令。这里`-i` 会替换 `curl -s https://ip.cn`。所以这里会直接运行 `-i`, 而 `-i` 本身不是命令。

#### 解决办法

当存在 `ENTRYPOINT` 后，`CMD` 的内容将会作为参数传给 `ENTRYPOINT`，而这里 `-i` 就是新的 `CMD。`

```text
FROM ubuntu:18.04
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT [ "curl", "-s", "https://ip.cn" ]
```

这个dockerfile 就等于运行 

```bash
$ docker run image-name curl -s https://ip.cn -i
```

#### 场景二：应用运行前的准备工作

启动容器就是启动主进程，但有些时候，启动主进程前，需要一些准备工作。

比如 `mysql` 类的数据库，可能需要一些数据库配置、初始化的工作，这些工作要在最终的 mysql 服务器运行之前解决。

在启动服务前还需要以 `root` 身份执行一些必要的准备工作，最后切换到服务用户身份启动服务, 考虑到安全性。

```text
FROM alpine:3.4
...
RUN addgroup -S redis && adduser -S -G redis redis
...
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 6379
CMD [ "redis-server" ]
```

docker-entrypoint.sh:

```bash
#!/bin/sh
...
# allow the container to be started with `--user`
if [ "$1" = 'redis-server' -a "$(id -u)" = '0' ]; then
chown -R redis .
exec su-exec redis "$0" "$@"
fi
exec "$@"
```

该脚本的内容就是根据CMD的内容来判断，如果是redis-server的话，则切换到redis用户身份启动服务器，否则依旧使用root身份执行

## CMD/ENTRYPOINT 总结

### Dockerfile 中至少要有一个

如果镜像中既没有指定 CMD 也没有指定 ENTRYPOINT 那么在启动容器时会报错。

### 同时使用 CMD 和 ENTRYPOINT 的情况

对于 CMD 和 ENTRYPOINT 的设计而言，多数情况下它们应该是单独使用的。当然，有一个例外是 CMD 为 ENTRYPOINT 提供默认的可选参数。

![](../../../.gitbook/assets/image%20%287%29.png)

## RUN, CMD, ENTRYPOINT 的区别

* RUN命令执行命令并创建新的镜像层，通常用于安装软件包
* CMD命令设置容器启动后默认执行的命令及其参数，但CMD设置的命令能够被`docker run`命令后面的命令行参数替换。该命令仅在您运行容器不指定命令时候运行。
* ENTRYPOINT配置容器启动时的执行命令（不会被忽略，一定会被执行，即使运行 `docker run`时指定了其他命令）

