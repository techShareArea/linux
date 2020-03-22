### 前言
linux葵花宝典

#### linux进程的五种状态
```
1. TASK_RUNNING(运行态): 进程是可执行的，或者正在执行，或者在运行队列中等待执行
2. TASK_INTERRUPTIBLE(可中断睡眠态): 进程被阻塞，等待某些条件的完成，一旦完成这些条件，内核就会将该进程的状态设置为运行态
3. TASK_UNINTERRUPTIBLE(不可中断睡眠态): 进程被阻塞，等待某些条件的完成；与可中断睡眠态不同的是，该进程状态不可被信号唤醒      
4. TASK_ZOMBIE(僵死态): 该进程已经结束，但是其父进程还没有将其回收
5. TASK_STOP(终止态): 进程停止执行，通常进程在收到SIGSTOP、SIGTTIN、SIGTTOU等信号的时候会进入该状态    
```

#### awk
##### Ⅰ.求某列的和
```
[root@cangqiong ~]# cat sum.txt 
1
2
```
> [root@cangqiong ~]# awk '{sum += $1};END {print sum}' sum.txt     
 
```
3
```

##### Ⅱ.求符合条件行的值，进行求和
```
[root@cangqiong ~]# cat sum.txt 
a 1
b 2
c 3
a 4
```
> [root@cangqiong ~]# awk '/a/ {sum += $2};END {print sum}' sum.txt 

```
5
```

#### linux文件系统
linux文件系统最基本的组成:bootfs和rootfs

##### bootfs
boot file system(bootfs)：包含boot loader和kernel。用户不会修改这个文件系统。实际上，在启动(boot)过程完成后，整个内核都会被加载进内存，此时bootfs会被卸载掉从而释放出所占用的内存。同时也可以看出，对于同样内核版本的不同的Linux发行版的bootfs都是一致的。

##### rootfs
root file system(rootfs)：包含典型的目录结构，包括/dev,/proc,/bin,/etc,/lib,/usr,/tmp等再加上要运行用户应用所需要的所有配置文件，二进制文件和库文件。这个文件系统在不同的Linux发行版中是不同的。而且用户可以对这个文件进行修改。

Linux系统在启动时，roofs首先会被挂载为只读模式，然后在启动完成后被修改为读写模式，随后它们就可以被修改了。

所有 Docker 容器都共享主机系统的 bootfs 即 Linux 内核
每个容器有自己的 rootfs，它来自不同的 Linux 发行版的基础镜像，包括 Ubuntu，Debian 和 SUSE 等
所有基于一种基础镜像的容器都共享这种 rootfs

##### aufs:(Ubuntu/debian默认)
AUFS(AnotherUnionFS)是一种Union FS，简单来说就是支持将不同目录挂载到同一个虚拟文件系统下的文件系统。Aufs driver是Docker最早支持的driver，但是aufs只是Linux内核的一个补丁集，而且不太可能会被加入到Linux内核中。但是由于aufs是唯一一个可以实现容器间共享可执行代码和运行库的storage driver，所以当你跑成千上百个拥有相同程序代码或者运行库的时候，aufs是个相当不错的选择。

##### device mapper：(CentOS/RH默认)
Device mapper是Linux 2.6内核中提供的一种从逻辑设备到物理设备的映射框架机制，在该机制下，用户可以很方便的根据自己的需要制定实现存储资源的管理策略。
Device mapper driver会创建一个100G的简单文件包含你的镜像和容器。每一个容器被限制在10G大小的卷内，可以调整。

