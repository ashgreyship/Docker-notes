---
description: 默认情况下，容器可以主动访问到外部网络的连接，但是外部网络无法访问到容器。
---

# 外部网络访问容器

`-P` 或 `-p` 参数可以指定端口映射。

## 随机端口/`-P` 

```bash
$ docker run -d -P training/webapp python app.py
```

使用 `docker container ls` 可以看到，本地主机的 49155 被映射到了容器的 5000 端口。此时访问本机的 49155 端口即可访999问容器内 web 应用提供的界面。

```bash
$ docker container ls -l
CONTAINER ID  IMAGE                   COMMAND       CREATED        STATUS        PORTS                    NAMES
bc533791f3f5  training/webapp:latest  python app.py 5 seconds ago  Up 2 seconds  0.0.0.0:49155->5000/tcp  nostalgic_morse

$ docker logs -f nostalgic_morse
* Running on http://0.0.0.0:5000/
```

## `-p`的使用

可以指定要映射的端口，并且，在一个指定端口上只可以绑定一个容器。

支持的格式：`ip:hostPort:containerPort | ip::containerPort | hostPort:containerPort`

### 映射到指定地址的指定端口

 `ip:hostPort:containerPort`  \#**指定ip、指定宿主机port、指定容器port**

```bash
$ docker run -d -p 127.0.0.1:5000:5000 training/webapp python app.py
```

### 映射到指定地址的任意端口

使用 `ip::containerPort` \#**指定ip、未指定宿主机port（随机）、指定容器port**

此例子绑定 localhost 的**任意端口**到容器的 5000 端口，本地主机会自动分配一个端口。

```bash
$ docker run -d -p 127.0.0.1::5000 training/webapp python app.py
```

### 映射所有接口地址

使用 `hostPort:containerPort`  \#**未指定ip、指定宿主机port、指定容器port**

此例子把本地的 5000 端口映射到容器的 5000 端口，可以执行

```bash
$ docker run -d -p 5000:5000 training/webapp python app.py
```

{% hint style="info" %}
`-p` 标记可以多次使用来绑定多个端口
{% endhint %}

```bash
$ docker run -d \
    -p 5000:5000 \
    -p 3000:80 \
    training/webapp \
    python app.py
```

