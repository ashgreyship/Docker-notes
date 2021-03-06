# 服务端防护

Docker 服务的运行目前需要 root 权限，因此其安全性十分关键。

Docker 允许用户在主机和容器间共享文件夹，同时不需要限制容器的访问权限，这就容易让容器突破资源限制。例如，恶意用户启动容器的时候将主机的根目录`/`映射到容器的 `/host` 目录中，那么容器理论上就可以对主机的文件系统进行任意修改了。

最近改进的 Linux 命名空间机制将可以实现使用非 root 用户来运行全功能的容器。这将从根本上解决了容器和主机之间共享文件系统而引起的安全问题

