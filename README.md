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




