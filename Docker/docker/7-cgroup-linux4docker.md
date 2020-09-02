# 2.关于Linux的Cgroups
## 概念
> Linux Cgroups(Control Groups)在Linux Namespace为进程隔离出一定空间的基础上为此进行的资源限制、控制以及统计的能力。资源包含有：**CPU**、**内存**、**存储**、**网络**等。通过Cgroups可以限制某个进程的资源占用、并且可以实时监控进程以及统计信息。

## Cgroups组件

+ **cgroup**:
    
+ **subsystem**:
    + blkio: `设置对块设备输入输出进行控制``
    + cpu:设置cgroup中进程的CPU调度策略
    + cpuacct:统计cgroup中进程CPU占用情况
    + cpuset:在多核机器上设置cgroup中进程可以使用的CPU和内存(内存仅适用于NUMA架构)
    + devices:
    + freezer:
    + memory:
    + net_cls:
    + net_prio:
    + ns:
+ **hierarchy**: