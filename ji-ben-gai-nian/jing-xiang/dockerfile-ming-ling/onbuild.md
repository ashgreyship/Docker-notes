# ONBUILD

用于指定当前的docker镜象用作另一个镜象的基本镜象时候运行的命令。

ONBUILD用于在Dockerfile中定义一个触发器，Dockerfile用于build镜像文件，此镜像文件可作为base image被另一个Dockerfile用作FROM指令的参数，并以之构建新的映像文件。

**ONBUILD不会在自己构建的时候执行，而是在被其他人使用作为基础镜像的时候才会执行。**

## 例子

### 基础镜像 （镜像名为 my-node）

```text
FROM node:slim
RUN mkdir /app
WORKDIR /app
ONBUILD COPY ./package.json /app
ONBUILD RUN [ "npm", "install" ]
ONBUILD COPY . /app/
CMD [ "npm", "start" ]
```

### 新镜像

```text
FROM my-node
...
...
```



