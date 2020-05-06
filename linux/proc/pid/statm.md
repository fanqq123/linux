# /proc/PID/statm

提供进程内存使用情况信息，以页为单位

## linux页大小查看

```shell
[root@manager-node ~]# getconf PAGE_SIZE
4096
```

## 实例

```shell
[fanqq@fanqq~]$ cat /proc/11902/statm
460996 98664 516 13750 0 436783 0
```

-  Size (total pages) 任务虚拟地址空间的大小 VmSize/4

- Resident(pages) 应用程序正在使用的物理内存的大小 VmRSS/4

- Shared(pages) 共享页数 0

- Trs(pages) 程序所拥有的可执行虚拟内存的大小 VmExe/4

- Lrs(pages) 被映像到任务的虚拟内存空间的库的大小 VmLib/4

- Drs(pages) 程序数据段和用户态的栈的大小 （VmData+ VmStk ）4

- dt(pages) 脏页数量
  