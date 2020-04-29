# strace

跟踪系统调用和信号

## 格式

```shell
用法: strace [-CdffhiqrtttTvVxxy] [-I n] [-e expr]...
              [-a column] [-o file] [-s strsize] [-P path]...
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]
   or: strace -c[df] [-I n] [-e expr]... [-O overhead] [-S sortby]
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]
```

## 参数

```shell
-c		计算每个系统调用的时间，调用和错误，并在程序退出时报告摘要
-C		与-c类似，但是在进程运行时也可以打印常规输出
-D		将跟踪程序进程作为独立的孙子运行，而不作为跟踪的父项运行
-d		在标准错误上显示strace本身的一些调试输出
-f		跟踪子进程，这些子进程是由于fork，vfork和clone系统调用而由当前跟踪的进程创建的
-ff		如果-o filename选项有效，则将每个进程跟踪写入filename.pid，其中pid是每个进程的数字进程ID
-h		打印帮助摘要
-i		在系统调用时打印指令指针
-q		禁止显示有关附加，分离等的消息。当输出重定向到文件并且直接运行命令而不是运行命令时，
		此消息自动发生附加
-qq		如果给出两次，则禁止显示有关进程退出状态的消息
-r		在进入每个系统调用时打印一个相对时间戳。这记录了连续系统调用开始之间的时间差
-t		在一天的时间前面加上跟踪的每一行
-tt		如果给出两次，则打印的时间将包括微秒
-ttt	如果给定三次，则打印的时间将包括微秒，并且前导部分将打印为自该纪元以来的秒数
-T		显示在系统调用中花费的时间。这将记录每个系统调用的开始和结束之间的时间差
-V		打印strace的版本号
-x		以十六进制字符串格式打印所有非ASCII字符串
-xx		以十六进制字符串格式打印所有字符串
-y		打印与文件描述符参数关联的路径
-a		列在特定列中对齐返回值（默认列40）
-e		expr一个限定表达式，它修改要跟踪的事件或如何跟踪它们的事件
		表达式的格式为：[qualifier =] [！] value1 [，value2] ...
		qualifier只能是 trace,abbrev,verbose,raw,signal,read,write其中之一，
		value是用来限定的符号或数字.默认的 qualifier是 trace.感叹号是否定符号，
		例如: -e open等价于 -e trace=open,表示只跟踪open调用，而-e trace!=open表示跟踪除了			open以外的其他调用，有两个特殊的符号 all 和 none都没有明显的含义
-e trace=file 只跟踪有关文件操作的系统调用. 
-e trace=process 只跟踪有关进程控制的系统调用. 
-e trace=network 跟踪与网络有关的所有系统调用. 
-e strace=signal 跟踪所有与系统信号有关的 系统调用 
-e trace=ipc   跟踪所有与进程通讯有关的系统调用 
-e abbrev=set 设定 strace输出的系统调用的结果集.-v 等与 abbrev=none.默认为abbrev=all. 
-e raw=set 	  将指定的系统调用的参数以十六进制显示. 
-e signal=set 指定跟踪的系统信号，默认为all，如 signal=!SIGIO(或者signal=!io),表示不跟踪SIGIO				  信号 
-e read=set 输出从指定文件中读出的数据.例如: -e read=3,5 -e write=set 
-o filename	将跟踪输出写入文件，如果使用-ff，则使用filename.pid
-O		开销将跟踪系统调用的开销设置为开销微秒。
		这对于覆盖默认启发式方法很有用，它可以猜测花费了多少时间
-p pid	使用进程ID pid附加到进程并开始跟踪。跟踪可以随时通过键盘中断信号（CTRL-C）终止
-P path	仅跟踪系统调用访问路径。多个-P选项可用于指定多个路径
-s strsize	指定要打印的最大字符串大小（默认为32）
-S sortby	按照指定的标准对-c选项打印的直方图的输出进行排序，可选值为time, calls, name
-u username	运行带有用户ID，组ID和用户名补充组的命令。该选项仅在以root用户身份运行时才有用，
			并且可以正确执行setuid和/或setgid二进制文件。除非使用此选项，
			否则setuid和setgid程序将在没有有效特权的情况下执行
-E var = val	在其环境变量列表中运行var = val的命令
-E var		在将其传递给命令之前，从继承的环境变量列表中删除var
```

## 实例

### 追踪vmstat命令数据来源

```shell
strace -e open vmstat
open("tls/x86_64/libprocps.so.4", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
open("tls/libprocps.so.4", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
open("x86_64/libprocps.so.4", O_RDONLY|O_CLOEXEC) = -1 ENOENT (No such file or directory)
.....
open("/proc/meminfo", O_RDONLY)         = 3
open("/proc/stat", O_RDONLY)            = 4
open("/proc/vmstat", O_RDONLY)          = 5
.....
```

> `open`是linux系统函数，通过上述例子我们可以看到`vmstat`展示的数据，其实是从`/proc/meminfo`，`stat`，`vmstat`文件里面读取的数据，加以处理以更直观的方式展现出来

### 程序因权限问题启动失败且无日志打印

```shell
strace -o trade.log ./statrtup.sh 
open("/home/sgeapp/apache-tomcat-8.5.54/logs/catalina.out", O_WRONLY|O_CREAT|O_APPEND, 0666) = -1 EACCES (Permission denied)
open("/home/sgeapp/apache-tomcat-8.5.54/logs/catalina.out", O_WRONLY|O_APPEND) = -1 EACCES (Permission denied)
open("/usr/share/locale/locale.alias", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=2502, ...}) = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f190fad0000
read(3, "# Locale name alias data base.\n#"..., 4096) = 2502
read(3, "", 4096)                       = 0
close(3)                                = 0
munmap(0x7f190fad0000, 4096)            = 0
open("/usr/share/locale/zh_CN/LC_MESSAGES/libc.mo", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=81139, ...}) = 0
mmap(NULL, 81139, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f190fab7000
close(3)                                = 0
open("/usr/share/locale/zh_CN/LC_MESSAGES/bash.mo", O_RDONLY) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=123267, ...}) = 0
mmap(NULL, 123267, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f190fa98000
close(3)                                = 0
fstat(2, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 0), ...}) = 0
mmap(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f190fad0000
write(2, "/home/sgeapp/apache-tomcat-8.5.5"..., 119/home/sgeapp/apache-tomcat-8.5.54/bin/catalina.sh:行484: /home/sgeapp/apache-tomcat-8.5.54/logs/catalina.out: 权限不够
) = 119
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
exit_group(1)                       
```

> 追踪tomcat程序启动，可以检查查看到那些文件没有权限操作

### 追踪已启动程序系统调用

```shell
strace -p PID
```

### 追踪进程子线程信息

```shell
strace -f -o java.txt -p PID
```

### 计算每个系统调用的时间，调用和错误

```shell
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 22.24    0.000688           8        90        60 open
 22.01    0.000681          16        43           mmap
 21.27    0.000658          47        14         7 wait4
  8.31    0.000257           6        42           read
  6.50    0.000201           4        47        31 stat
  3.49    0.000108          54         2           arch_prctl
  3.07    0.000095           4        24           mprotect
  2.78    0.000086          14         6         2 faccessat
  1.62    0.000050           1        42           close
  1.62    0.000050           0       443           rt_sigprocmask
  1.29    0.000040           6         7           clone
  1.07    0.000033           1        26           fstat
  1.07    0.000033           7         5           munmap
  1.00    0.000031           5         6         2 access
  0.87    0.000027           1        39           rt_sigaction
  0.52    0.000016           8         2           execve
  0.36    0.000011           1        12           brk
  0.19    0.000006           1        12           lseek
  0.16    0.000005           1         4           getrlimit
  0.13    0.000004           2         2           uname
  0.10    0.000003           2         2           getppid
  0.10    0.000003           2         2           getpgrp
  0.06    0.000002           1         2           getpid
  0.06    0.000002           0         6           getegid
  0.03    0.000001           0         7           rt_sigreturn
  0.03    0.000001           0         6           getuid
  0.03    0.000001           0         6           getgid
  0.03    0.000001           0         6           geteuid
  0.00    0.000000           0         6           write
  0.00    0.000000           0         2           lstat
  0.00    0.000000           0         3         2 ioctl
  0.00    0.000000           0         6           pipe
  0.00    0.000000           0         2           dup2
  0.00    0.000000           0         6         2 fcntl
  0.00    0.000000           0         1           umask
------ ----------- ----------- --------- --------- ----------------
100.00    0.003094                   931       106 total
```

