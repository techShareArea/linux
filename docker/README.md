### 前言
docker容器本质上是宿主机的进程，Docker通过namespace实现了资源隔离，通过cgroups实现了资源限制，通过写时复制机制（copy-on-write）实现了高效的文件操作。

#### namespace的6种隔离资源
```
namespace   系统调用参数      隔离内容
UTS         CLONE_NEWUTS    主机名或域名
IPC         CLONE_NEWIPC    信号量、消息队列和共享内存
PID         CLONE_NEWPID    进程编号
Network     CLONE_NEWNET    网络设备、网络栈、端口等
Mount       CLONE_NEWNS     挂载点(文件系统)
User        CLONE_NEWUSER   用户组和用户名
```

#### docker文件系统
docker文件系统分为两层:bootfs和rootfs。在内核启动之后，bootfs会被unmount掉。rootfs则包含了一般系统上的常见目录结构，类似于/dev,/proc,/bin,/etc,/var等一些基本的文件和目录

##### docker文件系统类型
docker在使用的文件系统包括但不限于以下这几种：aufs,device mapper,btrfs,overlayfs,vfs,zfs。aufs是ubuntu常用的，device mapper是centos，btrfs是SUSE，overlayfs ubuntu和centos都会使用，vfs和zfs常用在solaris系统。现在最新的docker版本中默认两个系统都是使用的overlay2。

##### rootfs实现原理:chroot
chroot(change root file system)，即改变进程的根目录到你指定的位置。

##### 联合文件系统：UniorFS
Union File System也叫UnionFS，最主要的功能是将多个不同位置的目录联合挂载(union mount)到同一个目录下。

##### overlayfs
Overlayfs是一种堆叠文件系统，它依赖并建立在其它的文件系统之上（例如ext4fs和xfs等等），并不直接参与磁盘空间结构的划分，仅仅将原来底层文件系统中不同的目录进行“合并”，然后向用户呈现。因此对于用户来说，它所见到的overlay文件系统根目录下的内容就来自挂载时所指定的不同目录的“合集”。














