# sar

收集，报告或保存系统活动信息。

## 格式

```shell
用法: sar [ 选项 ] [ <时间间隔> [ <次数> ] ]
[  -A  ] [ -B ] [ -b ] [ -C ] [ -d ] [ -H ] [ -h ] [ -p ] [ -q ] [ -R ] 
[ -r ] [ -S ] [ -t ] [ -u [ ALL ] ] [ -V ] [ -v ] [ -W ] [ -w ] [ -y ] 
[ -I { int [,...] | SUM | ALL |XALL } ] [ -P { cpu [,...] | ALL } ] 
[ -m { keyword [,...] | ALL } ] [ -n { keyword [,...] | ALL } ] 
[ -j { ID | LABEL | PATH | UUID | ... } ] 
[ -f [ filename ] | -o [ filename  ]| -[0-9]+ ] 
[ -i interval ] [ -s [ hh:mm:ss ] ] [ -e [ hh:mm:ss ] ] [ interval [ count ] ]
```

## 选项

```shell
-A		等效于指定-bBdHqrRSuvwWy -I SUM -I XALL -m ALL -n ALL -u ALL -P ALL
-B		分页状况
-b		I/O和传输速率信息状况
-d		块设备状况
-e		[ hh:mm:ss ]
		设置报告的结束时间，默认结束时间为18点，必须以24小时格式给出，读取或写入数据时可以使用-f或-o
-f		从文件中提取记录
-H		报告大页面利用率统计信息
-h		显示简短帮助信息，然后退出
-I		{ int [,...] | SUM | ALL | XALL }
-i		设置状态信息刷新的间隔时间
-j		{ ID | LABEL | PATH | UUID | ... }
-m 		{ keyword [,...] | ALL } 报告电源管理统计信息
-n 		{ keyword [,...] | ALL } 网络统计信息
-o		将数据以二进制形式保存在文件中
-P 		{ cpu [,...] | ALL }
		显示指定处理器的统计信息，ALL关键字将报告每个单独处理器的统计信息，多个处理器0代表第一个
-p		漂亮打印设备名称。将此选项与选项-d结合使用
-q		显示任务数和平均负载
-R		显示内存统计信息
-r		内存利用率统计信息
-S		交换空间利用率统计信息
-s		[hh：mm：ss]
		设置数据的开始时间，使sar命令提取指定时间或之后指定时间标记的记录。
		默认开始时间为08:00:00，必须以24小时格式给出，
		仅当从文件中读取数据时才可以使用此选项-f
-t		从每日数据文件中读取数据时，指示sar应该以数据文件创建者的原始本地时间显示时间戳
-u		[ALL] 报告CPU利用率。ALL关键字指示应显示所有CPU字段
-V		打印版本号，然后退出
-v		报告inode，文件和其他内核表的状态
-W		报告交换统计信息
-w		报告任务创建和系统切换活动
-y		报告TTY设备活动
```

## 网络

```shell
sar -n <关键词> [ <时间间隔> [ <次数> ] ]
关键字：
DEV(网卡)，EDEV(网卡错误)，
NFS(NFS客户端)，NFSD(NFS服务器)，
SOCK(Sockets套接字V4)，SOCK6(Sockets套接字V6)，
IP(IP流)，EIP(IP流错误)，
ICMP(ICMP流)，EICMP(ICMP流错误)，
TCP(TCP流)，ETCP(TCP流错误)，
UDP(UDP流v4)，UDP6(UDP流错误V6)，
IP6(IP6流)，EIP6(IP6流错误)，
ICMP6(ICMP6流)，EICMP6(ICMP6流错误)
```

### 统计数据包、网卡流量

```shell
[root@localhost dev]# sar -n DEV 1 
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 2020年04月10日 _x86_64_ (1 CPU)
04时36分50秒    IFACE  rxpck/s  txpck/s   rxkB/s   txkB/s  rxcmp/s  txcmp/s rxmcst/s
04时36分51秒       lo     0.00     0.00     0.00     0.00     0.00     0.00     0.00
04时36分51秒    ens33     3.06     1.02     0.24     0.20     0.00     0.00     0.00
```

- **IFACE**		网卡名称
- **rxpck/s **   每秒接收的数据包总数
- **txpck/s**     每秒传输的数据包总数
- **rxkB/s**      每秒接收的千字节总数
- **txkB/s**      每秒传输的千字节总数
- **rxcmp/s**   每秒接收的压缩数据包数
- **txcmp/s**    每秒传输的压缩数据包数
- **rxmcst/s**   每秒收到的组播数据包数

### 网络设备故障统计信息

```shell
[root@localhost dev]# sar -n EDEV 1 
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

04时56分42秒     IFACE   rxerr/s   txerr/s    coll/s  rxdrop/s  txdrop/s  txcarr/s  rxfram/s  rxfifo/s  txfifo/s
04时56分43秒        lo      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
04时56分43秒     ens33      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00      0.00
```

- **IFACE**		网卡名称
- **rxerr/s**     每秒收到的错误数据包总数
- **txerr/s**     传输数据包时每秒发生的错误总数
- **coll/s**        传输数据包时每秒发生的冲突数
- **rxdrop/s**  由于linux缓冲区空间不足，每秒丢弃的接收数据包数
- **txdrop/s**  由于linux缓冲区空间不足，每秒丢弃的传输数据包数
- **txcarr/s**    传输数据包时每秒发生的载波错误数
- **rxfram/s**   每秒在接收到的数据包上发生的帧对齐错误数
- **rxfifo/s**      每秒在接收到的数据包上发生的FIFO超限错误数
- **txfifo/s**      每秒传输的数据包发生的FIFO超限错误数

## **CPU**

```shell
sar -u [ <时间间隔> [ <次数> ] ]
sar -P { cpu [,...] | ALL }
sar -w 
```

### cpu利用率

**总利用率统计**

```shell
[root@localhost dev]# sar -u 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)
05时23分21秒     CPU     %user     %nice   %system   %iowait    %steal     %idle
05时23分22秒     all      0.00      0.00      1.00      0.00      0.00     99.00
05时23分23秒     all      0.00      0.00      0.00      0.00      0.00    100.00
05时23分24秒     all      0.00      0.00      1.01      0.00      0.00     98.99
05时23分25秒     all      1.01      0.00      0.00      0.00      0.00     98.99
```

**单个cpu利用率统计**

```shell
[root@localhost dev]# sar -P ALL 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

05时28分30秒     CPU     %user     %nice   %system   %iowait    %steal     %idle
05时28分31秒     all      0.00      0.00      1.01      0.00      0.00     98.99
05时28分31秒       0      0.00      0.00      1.01      0.00      0.00     98.99
```

- **CPU**		all 表示统计信息为所有 CPU 的平均值
- **%user**    用户空间占用CPU的百分比
- **%nice**     改变过优先级的进程占用CPU的百分比
- **%system**  内核空间占用CPU的百分比
- **%iowait**    IO等待占用CPU的百分比
- **%steal**      虚拟机管理程序从此虚拟机窃取的时间
- **%idle**       空闲CPU百分比
- **%irq**        一个或多个CPU服务硬件中断所花费的时间百分比
- **%soft**     一个或多个CPU服务软件中断所花费的时间百分比
- **%guest**   一个或多个CPU运行虚拟处理器所花费的时间百分比
- **%gnice**   一个或多个CPU运行一个好的guest虚拟机所花费的时间百分比

### 进程创建和上下文切换

```shell
[root@localhost dev]# sar -w 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

06时04分38秒    proc/s   cswch/s
06时04分39秒      0.00    342.42
06时04分40秒      0.00    337.76
06时04分41秒      0.00    337.37
06时04分42秒      0.00    350.00
06时04分43秒      0.00    343.43
```

- **proc/s**		每秒创建的进程总数
- **cswch/s**      每秒cpu上下文切换总数

### 进程数、平均负载

```shell
[root@localhost dev]# sar -q 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

06时40分08秒   runq-sz  plist-sz   ldavg-1   ldavg-5  ldavg-15   blocked
06时40分09秒         2       208      0.00      0.01      0.05         0
06时40分10秒         2       208      0.00      0.01      0.05         0
06时40分11秒         2       208      0.00      0.01      0.05         0
06时40分12秒         2       208      0.00      0.01      0.05         0
06时40分13秒         1       208      0.00      0.01      0.05         0
```

- **runq-sz**		正在运行任务数和等待运行的任务数
- **plist-sz**         任务列表中的任务数
- **ldavg-1**         最后一分钟的平均系统负载
- **ldavg-5**         过去5分钟的平均系统负载
- **ldavg-15**        过去15分钟的平均系统负载
- **blocked**         当前阻止的任务数量，等待I/O完成

## 内存

```shell
sar -r [ <时间间隔> [ <次数> ] ]
```

### 内存统计信息

```shell
[root@localhost dev]# sar -R 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

06时32分53秒   frmpg/s   bufpg/s   campg/s
06时32分54秒     -8.16      0.00      0.00
06时32分55秒      0.00      0.00      0.00
06时32分56秒      0.00      0.00      0.00
06时32分57秒      0.00      0.00      0.00
06时32分58秒      0.00      0.00      0.00
```

- **frmpg/s**		每秒系统释放的内存页数。负值表示系统分配的页面数
- **bufpg/s**        每秒系统用作缓冲区的其他内存页数。负值表示更少的页面被系统用作缓冲区
- **campg/s**       每秒系统缓存的其他内存页数。负值表示缓存中的页面较少

**注解：**获取Linux 内存页大小的命令：`getconf PAGE_SIZE`

### 内存使用率

```
[root@localhost dev]# sar -r 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

05时30分27秒 kbmemfree kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
05时30分28秒   2800164   1065360     27.56       932    337080   1024728     12.92    579404    197832        20
05时30分29秒   2800164   1065360     27.56       932    337080   1024728     12.92    579408    197832        20
05时30分30秒   2800164   1065360     27.56       932    337080   1024728     12.92    579416    197832        20
05时30分31秒   2800164   1065360     27.56       932    337080   1024728     12.92    579416    197832        20
```

- **kbmemfree**			可用内存量(单位：kb)
- **kbmemused**           已用内存量，这没有考虑内核本身使用的内存(单位：kb)
- **%memused**             已用内存的百分比
- **kbbuffers**                内核用作缓冲区的内存量(单位：kb)
- **kbcached**                内核用于缓存数据的内存量(单位：kb)
- **kbcommit**                当前工作负载所需的内存量(单位：kb)
- **%commit**                 当前工作负载所需的内存占内存总量（RAM + swap）的百分比
- **kbactive**                   活动内存量(单位：kb)
- **kbinact**                     不活动的内存量(单位：kb)
- **kbdirty**                      等待写回磁盘的内存量(单位：kb)

### 交换分区统计信息

```shell
[root@localhost dev]# sar -W 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

06时00分36秒  pswpin/s pswpout/s
06时00分37秒      0.00      0.00
06时00分38秒      0.00      0.00
06时00分39秒      0.00      0.00
06时00分40秒      0.00      0.00
```

- **pswpin/s**       每秒系统引入的交换页面总数
- **pswpout/s**     每秒系统带出的交换页面总数

### 交换分区利用率

```shell
[root@localhost dev]# sar -S 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

06时06分58秒 kbswpfree kbswpused  %swpused  kbswpcad   %swpcad
06时06分59秒   4064252         0      0.00         0      0.00
06时07分00秒   4064252         0      0.00         0      0.00
06时07分01秒   4064252         0      0.00         0      0.00
06时07分02秒   4064252         0      0.00         0      0.00
```

- **kbswpfree**		可用交换空间量
- **kbswpused**       已使用的交换空间量
- **%swpused**         已用交换空间的百分比
- **kbswpcad**          缓存的交换内存量
- **%swpcad**           缓存的交换内存相对于已使用交换空间量的百分比

### 内存分页统计信息

```shell
[root@localhost dev]# sar -B 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

07时33分28秒  pgpgin/s pgpgout/s   fault/s  majflt/s  pgfree/s pgscank/s pgscand/s pgsteal/s    %vmeff
07时33分29秒      0.00      0.00     56.57      0.00     23.23      0.00      0.00      0.00      0.00
07时33分30秒      0.00      0.00     26.53      0.00     21.43      0.00      0.00      0.00      0.00
07时33分31秒      0.00      0.00     17.17      0.00     22.22      0.00      0.00      0.00      0.00
07时33分32秒      0.00      0.00     17.17      0.00     21.21      0.00      0.00      0.00      0.00
```

- **pgpgin/s**           表示每秒从磁盘或SWAP置换到内存的字节数(KB)
- **pgpgout/s**        表示每秒从内存置换到磁盘或SWAP的字节数(KB)
- **fault/s**              每秒钟系统产生的缺页数，即主缺页与次缺页之和(major + minor)
- **majflt/s**            系统每秒发生的主缺页数，即需要从磁盘加载内存页的主要故障数
- **pgfree/s**           每秒系统在空闲列表中放置的页面数
- **pgscank/s**         kswapd守护程序每秒扫描的页面数
- **pgscand/s**         每秒直接扫描的页面数
- **pgsteal/s**           每秒系统已从缓存（页面缓存和swapcache）中回收的页面数，以满足其内存需求
- **%vmeff**              以pgsteal / pgscan计算，这是页面回收效率的度量        

### 索引节点，文件和其他内核表的状态

```shell
[root@localhost dev]# sar -v 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

05时47分30秒 dentunusd   file-nr  inode-nr    pty-nr
05时47分31秒     62990      1280     67786         2
05时47分32秒     62990      1280     67786         2
05时47分33秒     62990      1280     67786         2
05时47分34秒     62990      1280     67786         2
05时47分35秒     62990      1280     67786         2
```

- **dentunusd**		目录缓存中未使用的缓存条目数
- **file-nr**                 系统使用的文件句柄数
- **inode-nr**            系统使用的索引句柄数
- **pty-nr**                系统使用的伪终端数量

## 磁盘

```shell
[root@localhost dev]# sar -d -p 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

06时44分03秒  DEV  tps  rd_sec/s  wr_sec/s  avgrq-sz  avgqu-sz  await  svctm   %util
06时44分04秒  sda  0.00     0.00      0.00      0.00      0.00   0.00   0.00    0.00
```

- **DEV**	
- **tps**      每秒从物理磁盘 I/O 的次数。多个逻辑请求会被合并为一个 I/O 磁盘请求，一次传输的大小是不确定的
- **rd_sec/s**     从设备读取的扇区数，扇区的大小为512字节
- **wr_sec/s**    写入设备的扇区数，扇区的大小为512字节
- **avgrq-sz**    发出到设备的请求的平均大小（以扇区为单位）
- **avgqu-sz**    发出到设备的请求的平均队列长度
- **await**         发出给要服务的设备的I/O请求的平均时间（毫秒）
- **svctm**        发出给设备的I/O请求的平均服务时间（以毫秒为单位）
- **%util**         向设备发出I / O请求的经过时间百分比（设备的带宽利用率）

###  I/O 和传输速率信息

```shell
[root@localhost dev]# sar -b 1
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月10日 	_x86_64_	(1 CPU)

07时30分07秒       tps      rtps      wtps   bread/s   bwrtn/s
07时30分08秒      0.00      0.00      0.00      0.00      0.00
07时30分09秒      0.00      0.00      0.00      0.00      0.00
```

- **tps**    每秒钟物理设备的 I/O 传输总量
- **rtps**   每秒钟从物理设备读入的数据总量
- **wtps**   每秒钟向物理设备写入的数据总量
- **bread/s**     从设备读取的数据总量，以每秒块为单位。块等效于扇区，因此大小为512字节。
- **bwrtn/s**      每秒写入设备的数据总量（以块为单位）

## 读取、写入文件

```
sar -o test -u 1 100      将100次cpu利用率统计信息，以二进制格式写入当前目录的test文件中
sar -f test               将test里的数据读取展示
还可以使用sadf -d test命令将二进制数据转换成数据库可读的格式
或者将sadf转换的数据写入到csv文档中，然后绘制成表格
```

## 性能排查技巧

- 怀疑 CPU 存在瓶颈，可用`sar -u`和`sar -q`等来查看
- 怀疑内存存在瓶颈，可用`sar -B`、`sar -r`和`sar -W`等来查看
- 怀疑 I/O 存在瓶颈，可用`sar -b`、`sar -u`和`sar -d`等来查看