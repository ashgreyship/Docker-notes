# 列出镜像

```bash
$ docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
redis                latest              5f515359c7f8        5 days ago          183 MB
nginx                latest              05a60462f8ba        5 days ago          181 MB
mongo                3.2                 fe9198c04d62        5 days ago          342 MB
<none>               <none>              00285df0df87        5 days ago          342 MB
ubuntu               18.04               f753707788c5        4 weeks ago         127 MB
ubuntu               latest              f753707788c5        4 weeks ago         127 MB
```

## 镜像体积

{% hint style="danger" %}
使用命令列出的镜像所占空间和Docker Hub 上看到的镜像大小不同
{% endhint %}

Docker Hub 显示的是压缩后的体积。`Docker image ls`  显示的是镜像下载到本地后，展开的大小，准确说，是展开后的各层所占空间的总和.

{% hint style="danger" %}
`docker image ls` 列表中的镜像体积总和并非是所有镜像实际硬盘消耗
{% endhint %}

由于 Docker 镜像是多层存储结构，并且可以继承、复用，因此不同镜像可能会因为使用相同的基础镜像，从而拥有共同的层。由于 Docker 使用 Union FS，相同的层只需要保存一份即可，因此**实际镜像硬盘占用空间**很可能要比**这个列表镜像大小的总和**要小的多。

查看镜像、容器、数据卷所占用的空间

```text
$ docker system df
```

## 虚悬镜像 （dangling image）

```bash
<none>               <none>              00285df0df87        5 days ago          342 MB
```

仓库名和标签名都是`<none>` 的镜像是虚悬镜像。

这个镜像原本是有镜像名和标签的，原来为 `mongo:3.2`，随着官方镜像维护，发布了新版本后，重新 `docker pull mongo:3.2` 时，`mongo:3.2` 这个镜像名被转移到了新下载的镜像身上，而旧的镜像上的这个名称则被取消，从而成为了 `<none>`。除了 `docker pull` 可能导致这种情况，`docker build` 也同样可以导致这种现象。由于新旧镜像同名，旧镜像名称被取消，从而出现仓库名、标签均为 `<none>` 的镜像。

### 列出虚悬镜像

```bash
$ docker image ls -f dangling=true
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
<none>              <none>              00285df0df87        5 days ago          342 MB
```

### 删除虚悬镜像

```bash
$ docker image prune
```

## 中间层镜像

为了加速镜像构建、重复利用资源，Docker 会利用 **中间层镜像。**

默认的 `docker image ls` 列表中只会显示顶层镜像**。**

### **显示中间层镜像**

```bash
$ docker image ls -a
```

{% hint style="danger" %}
这样会看到很多无标签的镜像，与之前的虚悬镜像不同，这些无标签的镜像很多都是中间层镜像，是其它镜像所依赖的镜像。这些无标签镜像_不应该删除_，否则会导致上层镜像因为依赖丢失而出错。
{% endhint %}

### 列出部分镜像

#### 根据仓库名列出镜像

```bash
$ docker image ls ubuntu
```

#### 指定仓库名和标签

```bash
$ docker image ls ubuntu:18.04
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              18.04               f753707788c5        4 weeks ago         127 MB
```

#### 使用过滤器参数

比如，我们希望看到在 `mongo:3.2` 之后建立的镜像，可以用下面的命令：

```bash
$ docker image ls -f since=mongo:3.2
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
redis               latest              5f515359c7f8        5 days ago          183 MB
nginx               latest              05a60462f8ba        5 days ago          181 MB
```

使用`LABEL` 来过滤

```bash
$ docker image ls -f label=com.example.version=0.1
...
```

## 以特定格式显示

只显示镜像ID

```bash
$ docker image ls -q
5f515359c7f8
05a60462f8ba
fe9198c04d62
00285df0df87
f753707788c5
f753707788c5
1e0c3dd64ccd
```

ID可以交给 `docker image rm` 命令作为参数来删除指定的这些镜像。

使用 Go 的模板语法列出镜像结果，并且只包含镜像ID和仓库名：

```bash
$ docker image ls --format "{{.ID}}: {{.Repository}}"
5f515359c7f8: redis
05a60462f8ba: nginx
fe9198c04d62: mongo
00285df0df87: <none>
f753707788c5: ubuntu
f753707788c5: ubuntu
1e0c3dd64ccd: ubuntu
```



