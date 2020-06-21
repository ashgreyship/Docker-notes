# 容器互联

## 容器之间相互访问的两个要素:

* 容器的网络拓扑是否已经互联。默认情况下，所有容器都会被连接到 `docker0` 网桥上。
* 本地系统的防火墙软件 -- `iptables` 是否允许通过。默认为`ACCEPT`

因此：**默认情况下，不同容器之间是允许网络互通的。**

### 访问所有端口

当启动 Docker 服务（即 dockerd）的时候，默认会添加一条转发策略到本地主机 iptables 的 FORWARD 链上。

iptables的策略为通过（`ACCEPT`）还是禁止（`DROP`）取决于配置`--icc=true`（默认）还是 `--icc=false`。

如果手动指定 `--iptables=false` 则不会添加 `iptables` 规则。也可以在 `/etc/docker/daemon.json` 文件中配置 `{"icc": false}`设置iptables策略为`DROP`,禁止网络互通。

### 已有通信方式：

* 通过 IP地址来通信

  * 会导致ip地址的硬编码，不方便迁移，并且容器重启后ip地址会改变，除非使用固定的ip

* 通过宿主机的IP加上容器暴露出的端口号来通信
  * 通信方式比较单一，只能依靠监听在暴露出的端口的进程来进行有限的通信。

### 解决办法：

通过docker的link机制可以通过一个name来和另一个容器通信，link机制方便了容器去发现其它的容器并且可以安全的传递一些连接信息给其它的容器。

使用 network来分组隔离不同网络环境中的各组应用

## 使用network互联

### 网络模式\(Driver\)

#### Host: 

`host`相当于Vmware中的桥接模式，与宿主机在同一个网络中，但没有独立IP地址。可以让容器无需创建自己的网络协议栈，而直接访问宿主机的网络接口，在容器中执行ip addr会发现与宿主机的网络配置是一样的，host方式让容器直接使用宿主机的网络接口，传输数据的效率会更加高效，避免`bridge`方式带来的额外开销，但是这种方式也可以让容器访问宿主机的D-bus等网络服务，可能会带来**安全问题**

#### Container: 

这个模式指定新创建的容器和已经存在的一个容器共享一个Network Namespace，而不是和宿主机共享。新创建的容器不会创建自己的网卡，配置自己的IP，而是和一个指定的容器共享IP、端口范围等。同样，两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的。两个容器的进程可以通过lo网卡设备通信。

#### None：

该模式将容器放置在它自己的网络栈中，但是并不进行任何配置。在容器并不需要网络（例如只需要写磁盘卷的批处理任务）使用该模式关闭了容器的网络功能

#### Bridge: 

`Bridge`模式是Docker**默认的网络设置**，此模式会为每一个容器分配Network Namespace、设置IP等，并将一个主机上的Docker容器连接到一个虚拟的以太网桥

### 新建网络

```bash
 docker network create -d bridge <net-name1>
```

`-d` 参数指定 Docker 网络类型，有 `bridge` `overlay`。其中 `overlay` 网络类型用于 Swarm mode。

创建两个容器并连接到新建的网络

```bash
$ docker run -it --rm --name busybox1 --network <net-name1> busybox sh
$ docker run -it --rm --name busybox2 --network <net-name2> busybox sh
```

### 测试容器互联

```bash
# ping busybox2
PING busybox2 (172.19.0.3): 56 data bytes
64 bytes from 172.19.0.3: seq=0 ttl=64 time=0.072 ms
64 bytes from 172.19.0.3: seq=1 ttl=64 time=0.118 ms
```

```bash
 # ping busybox1
PING busybox1 (172.19.0.2): 56 data bytes
64 bytes from 172.19.0.2: seq=0 ttl=64 time=0.064 ms
64 bytes from 172.19.0.2: seq=1 ttl=64 time=0.143 ms
```

## 使用 --link互联

### 访问指定端口

在通过 `-icc=false` 关闭网络访问后，还可以通过 `--link=CONTAINER_NAME:ALIAS` 选项来访问容器的开放端口。

例如，在启动 Docker 服务时，可以同时使用 `icc=false --iptables=true` 参数来关闭允许相互的网络访问，并让 Docker 可以修改系统中的 `iptables` 规则。

当添加了 `--link=CONTAINER_NAME:ALIAS` 选项后，添加了 `iptables` 规则。

```bash
$ sudo iptables -nL
...
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  172.17.0.2           172.17.0.3           tcp spt:80
ACCEPT     tcp  --  172.17.0.3           172.17.0.2           tcp dpt:80
DROP       all  --  0.0.0.0/0            0.0.0.0/0
```



