# COPY & ADD

## COPY

`COPY` 指令将从构建上下文目录中 `<源路径>` 的文件/目录复制到新的一层的镜像内的 `<目标路径>` 位置。

* COPY \[--chown=&lt;user&gt;:&lt;group&gt;\] &lt;源路径&gt;... &lt;目标路径&gt;
* COPY \[--chown=&lt;user&gt;:&lt;group&gt;\] \["&lt;源路径1&gt;",... "&lt;目标路径&gt;"\]

## ADD

如果 `<源路径>` 为一个 `tar` 压缩文件的话，压缩格式为 `gzip`, `bzip2` 以及 `xz` 的情况下，`ADD` 指令将会自动解压缩这个压缩文件到 `<目标路径>` 去。

## 总结

在 Docker 官方的 Dockerfile 最佳实践文档 中要求，尽可能的使用 `COPY.`

ADD的最佳用途是将本地tar文件自动提取到镜像中。



