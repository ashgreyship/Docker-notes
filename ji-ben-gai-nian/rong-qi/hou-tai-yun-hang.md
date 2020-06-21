# 后台运行

后台运行又称守护态运行

使用参数 `-d`

## 例子

不使用 `-d` 参数运行容器：

```bash
$ docker run ubuntu:18.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
hello world
hello world
hello world
```

如果使用了 `-d` 参数运行容器。

```bash
$ docker run -d ubuntu:18.04 /bin/sh -c "while true; do echo hello world; sleep 1; done"
77b2dc01fe0f3f1265df143181e7b9af5e05279a884f4776ee75350ea9d8017a
```

前台运行：容器会把输出的结果 \(STDOUT\) 打印到宿主机上面

后台运行：输出结果可以用 `docker logs` 查看

{% hint style="info" %}
 容器是否会长久运行，是和 `docker run` 指定的命令有关，和 `-d` 参数无关。
{% endhint %}



