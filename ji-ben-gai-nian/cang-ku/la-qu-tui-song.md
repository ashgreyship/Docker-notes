# 拉取&推送

### 搜索镜像：

```text
docker search <keyword>
```

### 拉取镜像：

```text
docker pull <image-name>
```

### 推送镜像：

```text
docker push <username/image-name:tag>
```

### 自动构建：

允许用户通过 Docker Hub 指定跟踪一个目标网站（支持 [GitHub](https://github.com) 或 [BitBucket](https://bitbucket.org)）上的项目，一旦项目发生新的提交 （`commit`）或者创建了新的标签（`tag`），Docker Hub 会自动构建镜像并推送到 Docker Hub 中。



