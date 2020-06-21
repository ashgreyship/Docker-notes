# 命名空间 \(Namespace\)

docker engine 主要使用以下命名空间：

### **The `pid` namespace:** 

Process isolation \(PID: Process ID\).

不同用户的进程就是通过 pid 命名空间隔离开的，且不同命名空间中可以有相同 pid。

### **The `net` namespace:** 

Managing network interfaces \(NET: Networking\).

网络隔离是通过 net 命名空间实现的， 每个 net 命名空间有独立的 网络设备，IP 地址，路由表，/proc/net 目录。这样每个容器的网络就能隔离开来。Docker 默认采用 veth 的方式，将容器中的虚拟网卡同 host 上的一 个Docker 网桥 docker0 连接在一起。

### **The `ipc` namespace:** 

Managing access to IPC resources \(IPC: InterProcess Communication\).

容器中进程交互还是采用了 Linux 常见的进程间交互方法\(interprocess communication - IPC\)， 包括信号量、消息队列和共享内存等。然而同 VM 不同的是，容器的进程间交互实际上还是 host 上具有相同 pid 命名空间中的进程间交互，

### **The `mnt` namespace:** 

Managing filesystem mount points \(MNT: Mount\).

类似 chroot，将一个进程放到一个特定的目录执行。mnt 命名空间允许不同命名空间的进程看到的文件结构不同，这样每个命名空间 中的进程所看到的文件目录就被隔离开了

### **The `uts` namespace:** 

Isolating kernel and version identifiers. \(UTS: Unix Timesharing System\).

UTS\("UNIX Time-sharing System"\) 命名空间允许每个容器拥有独立的 hostname 和 domain name， 使其在网络上可以被视作一个独立的节点而非 主机上的一个进程

### **The `user` namespace:**

每个容器可以有不同的用户和组 id**,**也就是说可以在容器内用容器内部的用户执行程序而非主机上的用户。

