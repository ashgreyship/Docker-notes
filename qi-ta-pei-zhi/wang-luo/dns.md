# DNS

{% hint style="warning" %}
宿主机的 DNS 信息发送更新后，所有Docker 容器的DNS 会立即得到更新。
{% endhint %}

原因：Docker 利用了虚拟文件来挂载容器的3个相关配置文件，三个文件包括 `resolv.conf`, `/etc/hostname`, `/etc/hosts`

在容器中使用 `mount` 命令可以看到挂载信息

```bash
$ mount
/dev/disk/by-uuid/1fec...ebdf on /etc/hostname type ext4 ...
/dev/disk/by-uuid/1fec...ebdf on /etc/hosts type ext4 ...
tmpfs on /etc/resolv.conf type tmpfs ...
```

## 自定义容器的DNS 和主机名

在使用 `docker run` 命令启动容器时加入如下参数：

#### 主机名

`-h HOSTNAME` 或者 `--hostname=HOSTNAME` 设定容器的主机名，它会被写到容器内的 `/etc/hostname` 和 `/etc/hosts`。

#### DNS

`--dns=IP_ADDRESS` 添加 DNS 服务器到容器的 `/etc/resolv.conf` 中，让容器用这个服务器来解析所有不在 `/etc/hosts` 中的主机名。

#### 搜索域

`--dns-search=DOMAIN` 设定容器的搜索域，当设定搜索域为 `.example.com` 时，在搜索一个名为 host 的主机时，DNS 不仅搜索 host，还会搜索 `host.example.com`。

## 直接在容器里编辑网路配置文件

 可以容器中修改`/etc/hosts`, `/etc/hostname` 和 `/etc/resolv.conf` 文件, 但是这些修改是临时的，只在运行的容器中保留，容器终止或重启后并不会被保存下来，也不会被 `docker commit` 提交。

