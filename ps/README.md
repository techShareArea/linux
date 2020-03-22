### 前言
ps命令用于显示当前进程(process)的装填

#### 命令详解

##### ps -aux的输出详解
> [root@cangqiong ~]# ps -aux       

```
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.1  0.1  43676  3600 ?        Ss   Feb26  37:35 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
root         2  0.0  0.0      0     0 ?        S    Feb26   0:00 [kthreadd]
... ...
```
```
USER: 进程拥有着
PID: pid
%CPU: 占用的CPU使用率
%MEM: 占用的内存使用率
VSZ: 占用的虚拟内存大小
RSS: 占用的物理内存大小
TTY: 终端的次要装置号码
STAT: 该进程的状态(D:无法中断的休眠状态[通常I/O的进程];R:正在执行中;S:静止状态;T:暂停执行;Z:不存在但暂时无法执行;W:没有足够的内存可分配;<:高优先级的进程;N:低优先级的进程)
START: 进程开始时间
TIME: 执行的时间
COMMAND: 所执行的命令
```

##### ps -ef的输出详解
> [root@cangqiong ~]# ps -ef        

```
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 Feb26 ?        00:37:35 /usr/lib/systemd/systemd --switched-root --system --deserialize 22
root         2     0  0 Feb26 ?        00:00:00 [kthreadd]
```

```
UID: 用户ID/用户名
PID: 进程的ID
PPID: 父进程的ID
C: 进程占用CPU的百分比
STIME: 进程启动到现在的时间
TTY: 该进程在哪个终端上执行，若与终端无关，则显示?，若为pts/0等，则表示由网络连接主机进程
CMD: 命令的名称和参数
```



