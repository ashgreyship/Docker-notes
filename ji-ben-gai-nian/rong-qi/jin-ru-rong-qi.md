# 进入容器

两种方式可以进入容器：

* `docker attach` 
* `docker exec`

## `attach` 命令

`docker attach container-id`

```bash
$ docker run -dit ubuntu
243c32535da7d142fb0e6df616a3c3ada0b8ab417937c853a9e1c251f499f550

$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
243c32535da7        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           nostalgic_hypatia

$ docker attach 243c
root@243c32535da7:/#
```

## `exec` 命令

`docker exec -it  container-id bash`

```bash
$ docker run -dit ubuntu
69d137adef7a8a689cbcb059e94da5489d3cddd240ff675c640c8d96e84fe1f6

$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
69d137adef7a        ubuntu:latest       "/bin/bash"         18 seconds ago      Up 17 seconds                           zealous_swirles

$ docker exec -i 69d1 bash
ls
bin
boot
dev
...

$ docker exec -it 69d1 bash
root@69d137adef7a:/#
```

## 相同点

只能对已经在运行（后台运行）的容器使用

## 区别

在进入了某一个容器之后，使用 `exit`  命令：

如果之前是 `attach`命令启动容器，会退出终端并终止容器。

如果之前是 `exec`命令启动容器，会退出终端但**不会终止**容器。

