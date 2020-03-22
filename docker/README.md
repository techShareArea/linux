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




