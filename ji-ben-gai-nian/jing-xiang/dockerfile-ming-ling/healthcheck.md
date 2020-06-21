# HEALTHCHECK

`HEALTHCHECK` 健康检查

#### 格式：

* `HEALTHCHECK [选项] CMD <命令>`：设置检查容器健康状况的命令
* `HEALTHCHECK NONE`：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令

用来判断容器主进程的服务状态是否正常。比如程序进入死锁状态，或者死循环状态。

和 `CMD`, `ENTRYPOINT` 一样，`HEALTHCHECK` 只可以出现一次，如果写了多个，只有最后一个生效。

## 例子

```text
FROM nginx
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*
HEALTHCHECK --interval=5s --timeout=3s \
  CMD curl -fs http://localhost/ || exit 1
```

