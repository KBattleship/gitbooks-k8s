# 2.关于Linux的Cgroups
## 概念
> Linux Cgroups(Control Groups)在Linux Namespace为进程隔离出一定空间的基础上为此进行的资源限制、控制以及统计的能力。资源包含有：**CPU**、**内存**、**存储**、**网络**等。通过Cgroups可以限制某个进程的资源占用、并且可以实时监控进程以及统计信息。

## cgroups各模块

+ **cgroup**: 针对进程进行分组的一种策略机制。
    
    > 每一个cgroup中包含有一组进程。并且可以使用*subsystem*模块进行参数控制作用于此cgroup上的进程。
    
+ **subsystem**: 此模块对资源进行控制。

    > Ubuntu OS可以通过`apt install cgroup-bin`安装命令行工具，使用`lssubsys`查看Kernel所支持的subsystem list。

    + `blkio:` 设置对块设备输入输出进行控制
    + `cpu:` 设置cgroup中进程的CPU调度策略
    + `cpuacct:` 统计cgroup中进程CPU占用情况
    + `cpuset:` 在多核机器上设置cgroup中进程可以使用的CPU和内存(内存仅适用于NUMA架构)
    + `devices:` 控制cgroup中进程对设备的访问
    + `freezer:` 用于挂起(suspend)和恢复(resume)cgroup中的进程
    + `memory:` 限制cgroup中进程的内存占用
    + `net_cls:` 将cgroup中进程的网络包进行分类，以至于通过分类区区分不同cgroup中进程的网络包，并进行监控、限流等。
    + `net_prio:` 设置cgroup中进程产生的网络流量的优先级
    + `ns:` 使cgroup中进程在新的Namespace中fork出新进程(NEWNS)，同时创建出新的cgroup，并且此cgroup包含有新Namespace中的进程。
+ **hierarchy**: cgroup进程的继承关系。
    > 例如： 系统通过cgroup1针对一组定时任务进程进行CPU使用限制，同时其中一个进程还需要限制磁盘IO，这时候将可以通过cgroup2继承cgroup1限制CPU的同时增加磁盘IO限制。即cgroup2同时具有CPU、IO限制，并且不影响cgroup1组中其他进程的IO限制。

## cgroup各模块间关系
+ 系统创建hierarchy后，系统下所有进程都将被加入cgroup中，cgroup为根节点，被hierarchy创建的cgroup将被作为此cgroup根节点下的子节点;
+ subsystem与hierarchy是**1:1关系**(即，一个subsystem只能作用于一个hierarchy之上);
+ hierarchy与subsystem是**1:n关系**(即，一个hierarchy可以作用于多个subsystem之上);
+ 一个进程可以分布在不同的cgroup中，但需满足cgroup分布在不同的hierarchy中;
+ 一个进程fork出一个子进程的同时，子进程与父进程在同一cgroup中，但可以根据需求调整到其他cgroup中。