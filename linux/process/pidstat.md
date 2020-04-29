# pidstat

Linux任务的统计信息。

用于监视Linux内核当前正在管理的各个任务，它为使用以下命令选择的每个任务写入标准输出活动，可以用于监视选定任务的子进程

## 格式

```shell
用法: pidstat [ 选项 ] [ <时间间隔> [ <次数> ] ]
Options are:
[ -d ] [ -h ] [ -I ] [ -l ] [ -r ] [ -s ] [ -t ] [ -U [ <username> ] ] [ -u ]
[ -V ] [ -w ] [ -C <command> ] [ -p { <pid> [,...] | SELF | ALL } ]
[ -T { TASK | CHILD | ALL } ]
```

## 参数

```shell
-C		仅显示命令名称包含字符串comm的任务。该字符串可以是正则表达式
-d		报告I/O统计信息
-h		在一行水平显示所有信息，目的是让它易于被其他程序解析
-I
-l		显示进程命令名称及其所有参数
-p		{ pid [,...] | SELF | ALL }统计指定PID信息，pid是进程标识号
		--SELF关键字指示要报告统计信息，表示pidstat进程本身
		--ALL关键字表示要报告系统管理的所有任务的统计信息
-r		报告页面错误和内存利用率
-s		报告堆栈利用率
-T		{ TASK | CHILD | ALL }指定pidstat命令必须监视的内容
		--TASK关键字表示要报告单个任务的统计信息（这是默认选项）
		--CHILD关键字指示要针对所选任务及其所有子项全局报告统计信息
		--ALL关键字表示将针对单个任务报告统计信息，并针对选定任务及其子项报告全局统计信息
-t		显示与选定任务关联的线程信息
-U		显示指定用户的任务信息
-u		cpu利用率
-V		打印版本号，然后退出
-w		显示任务切换信息
```

## 实例

### I/O统计信息

```json
[root@localhost fanqq]# pidstat -d
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月11日 	_x86_64_	(1 CPU)

01时35分26秒   UID       PID   kB_rd/s   kB_wr/s kB_ccwr/s  Command
01时35分26秒     0         1      1.68      0.29      0.04  systemd
01时35分26秒     0       372      0.00      0.00      0.00  xfsaild/sda3
01时35分26秒     0       447      0.01      0.00      0.00  systemd-journal
01时35分26秒     0       458      0.00      0.00      0.00  lvmetad
01时35分26秒     0       465      0.12      0.00      0.00  systemd-udevd
01时35分26秒     0       559      0.02      0.01      0.00  auditd
01时35分26秒   998       580      0.06      0.00      0.00  polkitd
01时35分26秒     0       582      0.06      0.00      0.00  vmtoolsd
01时35分26秒     0       583      0.00      0.00      0.00  rngd
```

- **UID**			  任务的真实用户标识
- **PID**              进程PID
- **kB_rd/s**       每秒从磁盘读取的数据量（单位：kb）
- **kB_wr/s**       每秒写入磁盘的数据量（单位：kb）
- **kB_ccwr/s**     任务已取消其写入磁盘的数据量（单位：kb）
- **Command**     任务名称

### 内存利用率/页面错误数

```json
[root@localhost fanqq]# pidstat -r
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月11日 	_x86_64_	(1 CPU)

01时44分22秒   UID       PID  minflt/s  majflt/s     VSZ    RSS   %MEM  Command
01时44分22秒     0         1      0.15      0.00  125176   3624   0.09  systemd
01时44分22秒     0       447      0.07      0.00   36876   4064   0.11  systemd-journal
01时44分22秒     0       458      0.01      0.00  118952   1300   0.03  lvmetad
01时44分22秒     0       465      0.03      0.00   43948   2192   0.06  systemd-udevd
01时44分22秒     0       559      0.01      0.00   55476   1720   0.04  auditd
01时44分22秒   998       580      0.02      0.00  536320  10068   0.26  polkitd
01时44分22秒     0       582      0.58      0.00  302600   6376   0.16  vmtoolsd
```

- **minflt/s**	每秒任务执行的次要故障总数，即不需要从磁盘加载内存页面的次要故障总数
- **majflt/s**    每秒任务执行的主要故障总数，即需要从磁盘加载内存页面的主要故障总数
- **VSZ**           虚拟大小：整个任务的虚拟内存使用量（单位kb）
- **RSS**           长期内存使用，任务的不可交换物理内存的使用量（单位kb）
- **%MEM**      进程使用的物理内存百分比

### 堆栈利用率

```json
[root@localhost fanqq]# pidstat -s
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月11日 	_x86_64_	(1 CPU)

01时48分34秒   UID       PID StkSize  StkRef  Command
01时48分34秒     0         1    136      56  systemd
01时48分34秒     0       447    136      40  systemd-journal
01时48分34秒     0       458    136      12  lvmetad
01时48分34秒     0       465    136      36  systemd-udevd
01时48分34秒     0       559    136      48  auditd
01时48分34秒   998       580    136      24  polkitd
```

- **StkSize**       为任务保留为堆栈的内存量（单位kb），但不一定使用
- **StkRef**         任务引用的用作堆栈的内存量（单位kb）

### 任务关联线程信息

```json
[root@localhost fanqq]# pidstat -t -C mysql
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月11日 	_x86_64_	(1 CPU)

01时54分25秒   UID      TGID       TID    %usr %system  %guest    %CPU   CPU  Command
01时54分25秒    27      1505         -    0.38    0.45    0.00    0.82     0  mysqld
01时54分25秒    27         -      1505    0.00    0.00    0.00    0.00     0  |__mysqld
01时54分25秒    27         -      2137    0.00    0.00    0.00    0.01     0  |__mysqld
01时54分25秒    27         -      2138    0.00    0.00    0.00    0.00     0  |__mysqld
01时54分25秒    27         -      2139    0.00    0.00    0.00    0.00     0  |__mysqld
01时54分25秒    27         -      2140    0.00    0.00    0.00    0.00     0  |__mysqld
01时54分25秒    27         -      2141    0.00    0.00    0.00    0.00     0  |__mysqld
01时54分25秒    27         -      2142    0.00    0.00    0.00    0.00     0  |__mysqld
01时54分25秒    27         -      2143    0.00    0.00    0.00    0.00     0  |__mysqld
```

- **TGID**		线程组负责人的标识号
- **TID**          被监视线程的表示号

### cpu利用率

```json
[root@localhost fanqq]# pidstat -u 
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月11日 	_x86_64_	(1 CPU)

02时35分55秒   UID       PID    %usr %system  %guest    %CPU   CPU  Command
02时35分55秒     0         1    0.00    0.00    0.00    0.00     0  systemd
02时35分55秒     0         2    0.00    0.00    0.00    0.00     0  kthreadd
02时35分55秒     0         3    0.00    0.00    0.00    0.00     0  ksoftirqd/0
02时35分55秒     0         9    0.00    0.00    0.00    0.00     0  rcu_sched
02时35分55秒     0        10    0.00    0.00    0.00    0.00     0  watchdog/0
02时35分55秒     0        15    0.00    0.00    0.00    0.00     0  khungtaskd
```

- **CPU**        表示正在运行这个任务的处理器编号，0号是第一个

### 上下文切换

```json
[root@localhost fanqq]# pidstat -w
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月11日 	_x86_64_	(1 CPU)

02时39分39秒   UID       PID   cswch/s nvcswch/s  Command
02时39分39秒     0         1      0.05      0.03  systemd
02时39分39秒     0         2      0.00      0.00  kthreadd
02时39分39秒     0         3      0.40      0.00  ksoftirqd/0
02时39分39秒     0         7      0.00      0.00  migration/0
02时39分39秒     0         8      0.00      0.00  rcu_bh
02时39分39秒     0         9      1.06      0.00  rcu_sched
```

- **cswch/s**		每秒执行的任务的自愿上下文切换总数
- **nvcswch/s**    每秒执行的任务的非自愿上下文切换总数

### 单个任务统计信息

```json
[root@localhost fanqq]# pidstat -T TASK
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月11日 	_x86_64_	(1 CPU)

02时46分36秒   UID       PID    %usr %system  %guest    %CPU   CPU  Command
02时46分36秒     0         1    0.00    0.00    0.00    0.00     0  systemd
02时46分36秒     0         2    0.00    0.00    0.00    0.00     0  kthreadd
02时46分36秒     0         3    0.00    0.00    0.00    0.00     0  ksoftirqd/0
02时46分36秒     0         9    0.00    0.00    0.00    0.00     0  rcu_sched
02时46分36秒     0        10    0.00    0.00    0.00    0.00     0  watchdog/0
02时46分36秒     0        15    0.00    0.00    0.00    0.00     0  khungtaskd
02时46分36秒     0        28    0.00    0.00    0.00    0.00     0  khugepaged
02时46分36秒     0        94    0.00    0.00    0.00    0.00     0  kauditd
```

### 任务及其所有子项的的全局统计信息

```json
[root@localhost fanqq]# pidstat -T CHILD
Linux 3.10.0-514.el7.x86_64 (localhost.localdomain) 	2020年04月11日 	_x86_64_	(1 CPU)

02时43分27秒   UID       PID    usr-ms system-ms  guest-ms  Command
02时43分27秒     0         1      7250     12790         0  systemd
02时43分27秒     0         2         0        10         0  kthreadd
02时43分27秒     0         3         0      2310         0  ksoftirqd/0
02时43分27秒     0         9         0      1820         0  rcu_sched
02时43分27秒     0        10         0      1010         0  watchdog/0
02时43分27秒     0        15         0        50         0  khungtaskd
02时43分27秒     0        28         0       890         0  khugepaged
02时43分27秒     0        94         0        10         0  kauditd
```

- **usr-ms**      在用户级别（应用程序）执行时，任务及其所有子级花费的毫秒总数，具有或不具有优先级都包含在以内，该字段不包括运行虚拟处理器所花费的时间

- **system-ms**   在系统级别（内核）执行时，任务及其所有子代所花费的总毫秒数

- **guest-ms**      任务及其所有子代在虚拟机（运行虚拟处理器）中花费的总毫秒数

  