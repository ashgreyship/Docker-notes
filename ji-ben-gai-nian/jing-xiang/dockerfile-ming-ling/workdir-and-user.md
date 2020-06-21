# WORKDIR & USER

## WORKDIR

`WORKDIR` 指定工作目录

格式为 `WORKDIR <工作目录路径>`。

使用 `WORKDIR` 指令可以来指定工作目录（或者称为当前目录），以后**各层的当前目录**就被改为指定的目录

### 例子

```text
RUN cd /app
RUN echo "hello" > world.txt
```

这个 Dockerfile 无法修改 /app 路径下的 world.txt 文件。

因为每一个命令都会启动一个新的容器，然后执行命令，然后提交存储层文件变更。这两行 `RUN` 命令的执行环境根本不同。第一层 `RUN cd /app` 的执行仅仅是当前进程的工作目录变更，一个内存上的变化而已，其结果不会造成任何文件变更。而到第二层的时候，启动的是一个全新的容器，跟第一层的容器更完全没关系。

因此需要通过 WORKDIR 来改变工作目录。

## USER

USER 指定当前用户

格式：`USER <用户名>[:<用户组>]`

改变之后的层执行 `RUN`, `CMD` 以及 `ENTRYPOINT` 这类命令的身份。

### 例子

```text
RUN groupadd -r redis && useradd -r -g redis redis
USER redis
RUN [ "redis-server" ]
```

## WORKDIR 与 USER 比较

| WORKDIR | USER |
| :---: | :---: |
| **都是改变环境状态并影响以后的层** | \*\*\*\* |
| 切换到指定工作目录 | 切换到指定用户 |
| 目录不存在，会自动创建 | 用户必须是事先建立好的，否则无法切换 |

