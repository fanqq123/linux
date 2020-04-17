# vmstat

## 描述

vmstat虚拟内存统计，报告有关进程，内存，页面调度，块IO，磁盘和cpu等活动

## 格式

```shell
vmstat [-a] [-n] [-S unit] [delay [ count]]
vmstat [-s] [-n] [-S unit]
vmstat [-m] [-n] [delay [ count]]
vmstat [-d] [-n] [delay [ count]]
vmstat [-p disk partition] [-n] [delay [ count]]
vmstat [-f]
vmstat [-V]
```

## 常用参数

```shell
delay： 刷新时间间隔，不指定就只显示一条结果
count： 刷新次数，如果指定了刷新间隔，没有指定次数，将会刷新无数次
-a：    显示活动和非活动内存
-f：    显示从系统启动至今的fork数量，linux下创建进程的系统调用时fork
-m：	   显示slabinfo
-n:     只在开始时显示一次各字段名称
-s：    显示各种事件计数器和内存统计信息
-d：    报告磁盘统计信息
-D：    报告有关磁盘活动的一些摘要统计信息
-p：	   有关分区的详细统计信息
-S：    使用指定单位显示。参数有 k 、K 、m 、M，分别代表1000、1024、1000000、1048576字节（byte）。默认单位为K（1024 bytes）
-t：    将时间戳附加到每行
-w：    宽输出模式
-V：    显示版本信息
-h：	   显示帮助信息
```

## 默认模式

```shell
[sgeapp@localhost ~]$ vmstat 1
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 4  0      0 1468924    932 1795136    0    0     7     4  233  164  9  1 90  0  0
 0  0      0 1468908    932 1795136    0    0     0     0  137  330  1  0 99  0  0
 0  0      0 1468908    932 1795136    0    0     0     0  131  317  0  0 100  0  0
 0  0      0 1468908    932 1795136    0    0     0     0  133  320  0  1 99  0  0
 1  0      0 1468908    932 1795136    0    0     0     0  138  321  0  0 100  0  0
 2  0      0 1468908    932 1795136    0    0     0     0  132  310  0  0 100  0  0
```

- **procs**：
  - **r**：可运行的进程数（正在运行或正在等待运行时）
  - **b**：等待IO进程数
- **memory**：
  - **swpd**：使用的虚拟内存量
  - **free**：空闲内存量
  - **buff**：用作缓存区内存量
  - **cache**：用作缓存内存量
  - **inact**：不活跃内存量
  - **active**：活跃内存量
- **swap**：
  - **si**：从磁盘交换的内存量（内存从交换移至实际内存）
  - **so**：交换到磁盘的内存量（内存从实际内存移到交换）
- **io**：(现在的Linux版本块的大小为1024bytes)
  - **bi**：每秒读取的块数
  - **bo**：每秒写入的块数
- **system**：
  - **in**：每秒的中断数
  - **cs**：每秒上下文切换数
- **cpu**：
  - **us**：用户进程消耗的时间
  - **sy**：系统进程消耗的时间
  - **id**：空闲
  - **wa**：等待IO所花费的时间
  - **st**：从虚拟机窃取的时间

> 如果r值长期超过逻辑cpu核数，且id小于40，表示cpu负荷很重
> 如果bi、bo长期不等于0表示内存不足

## 磁盘模式

```shell
[sgeapp@localhost ~]$ vmstat  -d 1
disk- ------------reads------------ ------------writes----------- -----IO------
       total merged sectors      ms  total merged sectors      ms    cur    sec
sda    15875     31 2642592 2152768  27587   3226 1610204  198000      0    208
sr0        0      0       0       0      0      0       0       0      0      0
sda    15875     31 2642592 2152768  27587   3226 1610204  198000      0    208
sr0        0      0       0       0      0      0       0       0      0      0
sda    15875     31 2642592 2152768  27587   3226 1610204  198000      0    208
sr0        0      0       0       0      0      0       0       0      0      0
sda    15875     31 2642592 2152768  27587   3226 1610204  198000      0    208
sr0        0      0       0       0      0      0       0       0      0      0
```

- **reads**：
  - **total**：总读取成功完成
  - **merged**：分组读取(导致一个IO)
  - **sectors**：成功读取扇区
  - **ms**：花费的毫秒数
- **writes**：
  - **total**：总写入成功完成
  - **merged**：分组写入(导致一个IO)
  - **sectors**：成功写入扇区
  - **ms**：花费的毫秒数
- **IO**：
  - **cur**：I/O正在进行中
  - **sec**：IO的花费的秒数

## 磁盘分区模式

```shell
[sgeapp@localhost ~]$ vmstat -w -p sda 1
sda          reads   read sectors  writes    requested writes
                1280      52593       1045       4250
                1280      52593       1045       4250
                1280      52593       1045       4250
                1280      52593       1045       4250
                1280      52593       1045       4250
                1280      52593       1045       4250
```

- **reads**：此分区的读取总数
- **read sectors**：分区读取扇区总数
- **writes**：此分区的写入总数
- **requested writes**：分区写入扇区总数

## 磁盘总结信息

```shell
[sgeapp@localhost ~]$ vmstat -D
            2 disks 
            3 partitions 
        15875 total reads
           31 merged reads
      2642592 read sectors
      2152768 milli reading
        27595 writes
         3227 merged writes
      1610370 written sectors
       198068 milli writing
            0 inprogress IO
          208 milli spent IO
```

## slab模式

```shell
[root@localhost etc]# vmstat -w -m
Cache                       Num  Total   Size  Pages
AF_VSOCK                     30     30   1088     30
kcopyd_job                    0      0   3312      9
dm_uevent                     0      0   2608     12
dm_rq_target_io               0      0    136     60
xfs_dqtrx                     0      0    528     62
xfs_dquot                     0      0    472     69
```

- **cache**：缓存的名称
- **num**：当前活动对象的数量
- **total**：可用对象总数
- **size**：每个对象大小
- **pages**：具有至少一个活动对象的页面数

## 更多统计信息

```shell
[root@localhost etc]# vmstat -s
      3865524 K total memory
       598260 K used memory
       926756 K active memory
      1129324 K inactive memory
      1471188 K free memory
          932 K buffer memory
      1795144 K swap cache
      4064252 K total swap
            0 K used swap
      4064252 K free swap
      1724566 non-nice user cpu ticks
          233 nice user cpu ticks
       110796 system cpu ticks
     16671937 idle cpu ticks
         3669 IO-wait cpu ticks
            0 IRQ cpu ticks
         3403 softirq cpu ticks
            0 stolen cpu ticks
      1321296 pages paged in
       805265 pages paged out
            0 pages swapped in
            0 pages swapped out
     43013355 interrupts
     73469555 CPU context switches
   1586920856 boot time
        11097 forks
```

