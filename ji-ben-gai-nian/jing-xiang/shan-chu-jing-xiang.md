# 删除镜像

## 删除本地镜像

```bash
$ docker image rm [选项] <镜像1> [<镜像2> ...]
```

## 用 ID、镜像名、摘要删除镜像

其中，`<镜像>` 可以是 `镜像短 ID`、`镜像长 ID`、`镜像名` 或者 `镜像摘要`。

**长 ID:** 镜像的完整 ID

**短 ID:** `docker image ls` 默认列出的就已经是短 ID 了，一般取前3个字符以上，只要足够区分于别的镜像就可以了。

**镜像名字：**&lt;仓库名&gt;:&lt;标签&gt;

## Untagged 和 Deleted

删除行为分为两类

* Untagged
* Deleted

### 例子

```bash
$ docker image rm centos
Untagged: centos:latest
Untagged: centos@sha256:b2f9d1c0ff5f87a4743104d099a3d561002ac500db1b9bfa02a783a46e0d366c
Deleted: sha256:0584b3d2cf6d235ee310cf14b54667d889887b838d3f3d3033acd70fc3c48b8a
Deleted: sha256:97ca462ad9eeae25941546209454496e1d66749d53dfa2ee32bf1faabd239d38
```

当我们使用上面命令删除镜像的时候，实际上是在要求删除某个标签的镜像.

`Untagged` 是将满足我们要求的所有镜像标签都取消。

一个镜像可以对应多个标签。当我们删除了所指定的标签后，可能还有别的标签指向了这个镜像，如果是这种情况，那么 `Delete` 行为就不会发生。

{% hint style="warning" %}
并非所有的 `docker image rm` 都会产生删除镜像的行为，有可能仅仅是取消了某个标签而已。
{% endhint %}

当该镜像所有的标签都被取消了，该镜像很可能会失去了存在的意义，因此会触发删除行为。

{% hint style="info" %}
为什么明明没有别的标签指向这个镜像，但是它还是存在？

为什么有时候会发现所删除的层数和自己 `docker pull` 看到的层数不一样？
{% endhint %}

镜像是多层存储结构，因此在删除的时候也是从上层向基础层方向依次进行判断删除。很有可能某个其它镜像正依赖于当前镜像的某一层。这种情况，依旧不会触发删除该层的行为。直到没有任何层依赖当前层时，才会真实的删除当前层。

{% hint style="info" %}
如果容器存在（_即使不在运行_），能不能删除相关的镜像？
{% endhint %}

**不能**。容器对镜像有依赖。容器是以镜像为基础，再加一层容器存储层，组成这样的多层存储结构去运行的。因此该镜像如果被这个容器所依赖的，那么删除必然会导致故障。

## 用 docker image ls 命令来配合

```bash
$ docker image rm $(docker image ls -q redis)
```

```bash
$ docker image rm $(docker image ls -q -f before=mongo:3.2)
```







