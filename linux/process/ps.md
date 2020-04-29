# ps

显示有关活动进程选择的信息

## 格式

```shell
ps [options]
常用选项：
ps -e
ps -ef
ps -eF
ps -ely
```

## 参数

```shell
a			此选项导致ps列出带有终端(TTY)的所有进程，或在与x选项一起使用时列出所有进程。
-A|-e		显示所有进程
-N|--deselect	反向选择不符合条件的
-p|--pid|p	按进程ID选择
--ppid		按进程父ID选择
-a			显示所有终端机下执行的进程，除了阶段作业领导者之外
-d			选择除会话领导者之外的所有进程
r			显示仅正在运行的进程
x			显示所有进程，不以终端机来区分
-C			通过命令名称选择
-G			按真实组ID或名称选择
-g			通过会话或有效组名选择
-q|--quick-pid	通过进程ID选择（快速模式）
-s|--sid	通过会话ID选择
T			显示与该终端关联的所有进程
-t|ttylist	通过tty选择
u			通过有效的用户ID（EUID）或名称进行选择。这将选择有效用户名或ID在用户列表中的进程
-U			按真实用户ID（RUID）或名称选择。它选择真实用户名或ID在用户列表中的进程
-u			通过有效的用户ID（EUID）或名称进行选择。这将选择有效用户名或ID在用户列表中的进程

-c			显示-l选项的不同调度程序信息
--context	显示安全上下文格式（用于SELinux）
-f|-F		输出完整的格式。它还会导致输出命令参数。当与-L一起使用时，将添加NLWP(线程数)和LWP(线程ID)列
j			BSD作业控制格式
-j			作业格式
l			显示BSD长格式
-l			长格式，-y选项通常对此很有用
-M|Z		添加一列安全性数据
O			O选项的作用类似于-O（带有一些预定义公共字段的用户定义输出格式）
-O			与-o类似，但预加载了一些默认列
o			指定用户定义的格式。与-o和--format相同
-o			用户自定义输出的格式，format是单个参数，格式为空格分隔或逗号分隔的列表，它提供了一种指定单个输出列的方法
s			显示信号格式
u			显示面向用户的格式
v			显示虚拟内存格式
X			寄存器格式
-y			不显示标志；显示rss代替addr。此选项只能与-l一起使用

e			在命令后显示环境。
f|--forest	树状结构显示进程、子进程
h			无标题显示
-H			显示进程层次结构
k			指定排序顺序，等价于“--sort”
S			汇总一些信息，例如从死掉的子进程到其父进程的CPU使用率
H			将线程显示为进程
-m|m			在进程之后显示线程
-T			显示线程，可能带有SPID列
-L			显示线程，可能带有LWP和NLWP列
```

## 进程状态码

- **D**不间断的睡眠（通常是I/O）
- **R**正在运行或可运行（在运行队列上）
- **S**可中断睡眠（等待事件完成）
- **T**被作业控制信号停止
- **t**在跟踪过程中被调试器停止
- **X**已死（永远不会被看到）
- **Z**失效，僵尸进程

对于BSD格式，当时用stat关键字时，可能会显示其他字符：

- **<**，高优先级(对其他用户不好)
- **N**，低优先级(对其他用户很好)
- **L**，将页面锁定在内存中(用于实时和自定义IO)
- **s**，是会话
- **l**，是多线程的
- **+**，在前台进程组中

## **AIX格式描述符**

这个`ps`支持`AIX`格式描述符，它们的工作方式有点像`printf(1)`和`printf(3)`的格式代码。

例如，正常的默认输出可以这样产生：`ps -eo "%p %y %x %c`

| **CODE** | **NORMAL** | **HEADER** |
| :------: | :--------: | :--------: |
|    %C    |    pcpu    |    %CPU    |
|    %G    |   group    |   GROUP    |
|    %P    |    ppid    |    PPID    |
|    %U    |    user    |    USER    |
|    %a    |    args    |  COMMAND   |
|    %c    |    comm    |  COMMAND   |
|    %g    |   rgroup   |   RGROUP   |
|    %n    |    nice    |     NI     |
|    %p    |    pid     |    PID     |
|    %r    |    pgid    |    PGID    |
|    %t    |   etime    |  ELAPSED   |
|    %u    |   ruser    |   RUSER    |
|    %x    |    time    |    TIME    |
|    %y    |    tty     |    TTY     |
|    %z    |    vsz     |    VSZ     |

## 标准格式说明符

以下是用于控制输出格式(例如，使用选项-o)或使用GNU样式的“--sort”序选项对所选进程进行排序的不同关键字。例如:`ps -eo pid,user,args --sort user`,这个版本的ps试图识别大多数在ps的其他实现中使用的关键字。以下用户定义的格式说明符可能包含空格：**args, cmd, comm,command, fname, ucmd, ucomm, lstart, bsdstart, start**。某些关键字可能无法用于排序

| CODE       | HEADER  | 说明                                                         |
| :--------- | :------ | ------------------------------------------------------------ |
| %cpu       | %CPU    | 进程的CPU利用率为“#.#”格式。当前，它是CPU时间除以进程运行的时间(cputime/realtime比率)，表示为百分比。除非你是幸运的，否则它不会达到100%。(别名pcpu) |
| %mem       | %MEM    | 进程的驻留集大小与机器上物理内存的比率，以百分比表示。(别名PMEM) |
| args       | COMMAND | 命令，它的所有参数都是字符串。可以显示对参数的修改。该列中的输出可能包含空格。标记为“已失效”的进程部分死亡，等待其父进程完全销毁。有时进程args将不可用；当发生这种情况时，ps将可执行文件的名称打印在括号中。(别名cmd，命令)。当最后指定该列时，该列将扩展到显示的边缘。如果ps不能确定显示宽度，例如当输出被重定向(管道)到一个文件或另一个命令时，输出宽度是未定义的。(它可以是80，无限，TERM等决定)环境变量COLUMNS或-cols选项可以用于精确地确定这种情况下的宽度。w或-w选项也可用于调整宽度。 |
| blocked    | BLOCKED | blocked信号掩码。根据字段的宽度，以十六进制格式显示32位或64位掩码。(别名sig_block,  sigmask)。 |
| bsdstart   | START   | 命令开始的时间。如果进程在24小时前启动，则输出格式为“hh：mm”，否则为“mmm  dd”(其中mmm是月份的三个字母)。 |
| bsdtime    | TIME    | 用户和系统的累积CPU时间，。显示格式通常为“mmm：ss”，但如果进程占用的cpu时间超过999分钟，则可以移到右边。 |
| c          | C       | 处理器利用率当前，这是进程生存期内使用百分比的整数值。(见%cpu)。 |
| caught     | CAUGHT  | 捕获信号的掩码，见信号(7)。根据字段的宽度，以十六进制格式显示32或64位掩码。(别名sig_catch, sigcatch) |
| cgroup     | CGROUP  | 显示进程所属的控制组。                                       |
| class      | CLS     | 进程的调度类。(别名policy, cls)。字段的可能值是：     - not reported     TS SCHED_OTHER     FF SCHED_FIFO     RR SCHED_RR     B  SCHED_BATCH     ISO SCHED_ISO     IDL SCHED_IDLE     ?  unknown value |
| cls        | CLS     | 同class                                                      |
| cmd        | CMD     | 同args                                                       |
| comm       | COMMAND | 命令名(只有可执行的名称)。将不会显示对命令名的修改。标记为“已失效”的进程部分死亡，等待其父进程完全销毁。该列中的输出可能包含空格。(别名ucmd，ucomm)。     最后指定时，此列将延伸到显示的边缘。如果ps无法确定显示宽度，如将输出重定向（管道）到                     文件或其他命令，输出宽度是不确定的（可以是80，不受限制，由TERM变量确定，依此类推）。COLUMNS环境                     在这种情况下，可以使用variable或--cols选项精确确定宽度。w或-w选项也可用于调整宽度。 |
| command    | COMMAND | 同args                                                       |
| cp         | CP      | CPU使用率/ms                                                 |
| cputime    | TIME    | 累计CPU时间，"[DD-]HH:MM:SS"格式。(别名time)。               |
| egid       | EGID    | 进程的有效组ID数为十进制整数。(别名gid)。                    |
| egroup     | EFROUP  | 进程的有效组ID。如果可以获得并且字段宽度允许，这将是文本组ID，否则将是十进制表示。(别名group)。 |
| eip        | EIP     | 指令指针                                                     |
| esp        | ESP     | 栈指针                                                       |
| etime      | ELAPSED | 自进程启动以来，以[dd-]hh：]mm：SS形式运行的时间。           |
| euid       | EUID    | 有效用户ID，别名uid                                          |
| euser      | EUSER   | 有效用户名。如果可以获得并且字段宽度允许，这将是文本用户ID，否则将是十进制表示。n选项可用于强制十进制表示。(别名uname，user)。 |
| f          | F       | 与进程关联的标志，请参阅流程标志部分。(别名flag, flags)。    |
| fgid       | FGID    | 文件系统访问组ID。(别名fsgid)。                              |
| fgroup     | FGROUP  | 文件系统访问组ID。如果可以获得并且字段宽度允许，这将是文本用户ID，否则将是十进制表示。(别名fsgroup) |
| flag       | F       | 同f                                                          |
| flags      | F       | 同f                                                          |
| fname      | COMMAND | 进程可执行文件的基名的前8个字节。该列中的输出可能包含空格。  |
| fuid       | FUID    | 文件系统访问用户ID。(别名fsuid)。                            |
| fuser      | FUSER   | 文件系统访问用户ID。如果可以获得并且字段宽度允许，这将是文本用户ID，否则将是十进制表示。 |
| gid        | GID     | 同egid                                                       |
| group      | GROUP   | 同egroup                                                     |
| ignored    | IGNORED | 被忽略的信号的掩码，根据字段的宽度，以十六进制格式显示32位或64位掩码。(别名sig_ignore, sigignore) |
| label      | LABEL   | 安全标签，最常用于SELinux上下文数据。这是针对在高安全系统上发现的强制访问控制(“MAC”)。 |
| lstart     | STARTED | 命令开始的时间。                                             |
| lwp        | LWP     | 正在报告的LWP(轻量过程或线程)ID。(别名spid，tid)             |
| ni         | NI      | nice值，范围从19(最好)到-20(对他人不友好)。 (别名nice)。     |
| nice       | NI      | 同ni                                                         |
| nlwp       | NLWP    | 进程中的lwps(线程)数。(别名thcount)。                        |
| nwchan     | WCHAN   | 进程处于休眠状态的内核函数的地址(如果需要内核函数名称，请使用wchan)。正在运行的任务将在本列中显示一个破折号(‘-’)。 |
| pcpu       | %CPU    | 同%cpu                                                       |
| pending    | PENDING | 挂起信号的掩码。进程上挂起的信号不同于单个线程上的待决信号。使用m选项或-m选项查看两者。根据字段的宽度，以十六进制格式显示32位或64位掩码。(别名sig)。 |
| pgid       | PGID    | 进程组ID或相应的流程组领导的进程ID。(别名pgrp)。             |
| pgrp       | PGRP    | 同pgid                                                       |
| pid        | PID     | 进程的进程ID号                                               |
| pmem       | %MEM    | 同%mem                                                       |
| policy     | POL     | 同cls                                                        |
| ppid       | PPID    | 父进程id                                                     |
| psr        | PSR     | 进程当前分配给的处理器。                                     |
| rgid       | RGID    | 真实的组id                                                   |
| rgroup     | RGROUP  | 真正的组名。如果可以获得并且字段宽度允许，这将是文本组ID，否则将是十进制表示。 |
| rip        | RIP     | 64位指令指针。                                               |
| rsp        | RSP     | 64位栈指针。                                                 |
| rss        | RSS     | 驻留集大小，任务使用的非交换物理内存(以千字节为单位)。(别名rssize，rsz)。 |
| rssize     | RSS     | 同rss                                                        |
| rsz        | RSZ     | 同rss                                                        |
| rtprio     | RTPRIO  | 实时优先级                                                   |
| ruid       | RUID    | 实际用户ID                                                   |
| ruser      | RUSER   | 真实的用户ID。如果可以获得并且字段宽度允许，这将是文本用户ID，否则将是十进制表示。 |
| s          | S       | 最小状态显示(一个字符)。                                     |
| sched      | SCH     | 进程的调度策略。策略SCHED_OTHER(SCHED_Normal)、SCHED_FIFO、SCHED_RR、SCHED_BATCH、SCHED_ISO和SCHED_IDELL分别显示为0、1、2、3、4和5。 |
| sess       | SESS    | 会话ID或等效的会话领导的进程ID。(别名session，sid)。         |
| sgi_p      | P       | 进程当前正在执行的处理器。如果进程当前未运行或无法运行，则显示“*”。 |
| sgid       | SGID    | 保存的组ID。(别名svgid)                                      |
| sgroup     | SGROUP  | 保存的组名。如果可以获得并且字段宽度允许，这将是文本组ID，否则将是十进制表示。 |
| sid        | SID     | 同sess                                                       |
| sig        | PENDING | 同pending                                                    |
| sigcatch   | CAUGHT  | 同caught                                                     |
| sigignore  | IGNORED | 同ignored                                                    |
| sigmask    | BLOCKED | 同blocked                                                    |
| size       | SZ      | 如果进程要脏所有可写页，然后交换掉，则需要交换大约的交换空间。这个数字很粗糙！ |
| spid       | SPID    | 同lwp                                                        |
| stackp     | STACKP  | 进程堆栈的底部(开始)地址                                     |
| start      | STARTED | 命令开始的时候。如果进程在24小时前启动，则输出格式为“hh：mm：ss”，否则为“mmm  dd”(其中mmm是三个字母的月份名称)。 |
| start_time | START   | 进程的开始时间或日期。只有进程未启动的年份(即调用ps的年份)或“mmmdd”(如果进程未在同一天启动)或“hh：mm”将显示。 |
| stat       | STAT    | 多字符进程状态。有关不同值的含义，请参见处理状态代码一节。如果只希望显示第一个字符，请参见s和state。 |
| state      | S       | 同s                                                          |
| suid       | SUID    | 保存的用户ID。(别名svuid)。                                  |
| suser      | SUSER   | 保存的用户名。如果可以获得并且字段宽度允许，这将是文本用户ID，否则将是十进制表示。(别名svuser) |
| svgid      | SVGID   | 同sgid                                                       |
| svuid      | SVUID   | 同suid                                                       |
| sz         | SZ      | 进程核心图像的物理页面大小。这包括文本、数据和堆栈空间。当前排除了设备映射；这可能会发生更改。参见vsz和rss。 |
| thcount    | THCNT   | 同nlwp                                                       |
| tid        | TID     | 同lwp                                                        |
| time       | TIME    | 统计CPU时间,"[DD-]HH:MM:SS"格式。(别名cputime)。             |
| tname      | TTY     | 控制TY(终端)(别名tt，tty)。                                  |
| tpgid      | TPGID   | 进程连接到的TTY(终端)上的前台进程组的ID，如果进程没有连接到TTY，则为-1。 |
| tt         | TT      | 同tname。                                                    |
| tty        | TT      | 同tname。                                                    |
| ucmd       | CMD     | 同comm。                                                     |
| ucomm      | COMMAND | 同comm。                                                     |
| uid        | UID     | 同euid。                                                     |
| uname      | USER    | 同euser。                                                    |
| user       | USER    | 同euser。                                                    |
| vsize      | VSZ     | 同vsz。                                                      |
| vsz        | VSZ     | 进程的虚拟内存大小(1024字节单位)。当前排除了设备映射；这可能会发生更改。(别名vsize)。 |
| wchan      | WHAN    | 进程处于休眠状态的内核函数的名称，如果进程正在运行，则为“-”，如果进程是多线程且ps不显示线程，则为“*”。 |

## 实例

### 显示所有进程信息

```shell
[root@localhost ~]# ps -A
   PID TTY          TIME CMD
     1 ?        00:00:01 systemd
     2 ?        00:00:00 kthreadd
     3 ?        00:00:00 ksoftirqd/0
     7 ?        00:00:00 migration/0
     8 ?        00:00:00 rcu_bh
     9 ?        00:00:00 rcu_sched
    10 ?        00:00:00 watchdog/0
```

### 显示指定用户进程信息

```shell
[root@localhost ~]# ps -u root
   PID TTY          TIME CMD
     1 ?        00:00:01 systemd
     2 ?        00:00:00 kthreadd
     3 ?        00:00:00 ksoftirqd/0
     7 ?        00:00:00 migration/0
     8 ?        00:00:00 rcu_bh
     9 ?        00:00:00 rcu_sched
    10 ?        00:00:00 watchdog/0
    12 ?        00:00:00 khelper
```

### 显示所以进程信息，连同命令行

```shell
[root@localhost ~]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 4月12 ?       00:00:01 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root          2      0  0 4月12 ?       00:00:00 [kthreadd]
root          3      2  0 4月12 ?       00:00:00 [ksoftirqd/0]
root          7      2  0 4月12 ?       00:00:00 [migration/0]
root          8      2  0 4月12 ?       00:00:00 [rcu_bh]
root          9      2  0 4月12 ?       00:00:00 [rcu_sched]
root         10      2  0 4月12 ?       00:00:00 [watchdog/0]
root         12      2  0 4月12 ?       00:00:00 [khelper]
```

### ps 与grep 常用组合用法，查找特定进程

```shell
[root@localhost ~]# ps -ef|grep mysqld
mysql      1205      1  0 4月12 ?       00:02:25 /usr/sbin/mysqld
root       4223   2309  0 00:02 pts/1    00:00:00 grep --color=auto mysqld
```

### 输出指定字段信息（例如进程启动时间）

```shell
[root@localhost ~]# ps -C mysqld -o lstart|grep -v STARTED
                 STARTED
Sun Apr 12 18:48:16 2020
在配合使用date进行格式转换成易于查看的时间格式
[root@localhost ~]# date -d 'Sun Apr 12 18:48:16 2020' +'%F %T'
2020-04-12 18:48:16
```

### 查杀进程

```shell
ps -C mysqld -o pid=|xargs kill -9
```

### 列出目前所有的正在内存当中的程序

```shell
[root@localhost ~]# ps -aux
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root          1  0.0  0.0 125160  3624 ?        Ss   4月12   0:02 /usr/lib/systemd/systemd --switched-root --system --deserialize 21
root          2  0.0  0.0      0     0 ?        S    4月12   0:00 [kthreadd]
root          3  0.0  0.0      0     0 ?        S    4月12   0:00 [ksoftirqd/0]
root          7  0.0  0.0      0     0 ?        S    4月12   0:00 [migration/0]
root          8  0.0  0.0      0     0 ?        S    4月12   0:00 [rcu_bh]
root          9  0.0  0.0      0     0 ?        R    4月12   0:00 [rcu_sched]
```

- **VSZ**：process 使用掉的虚拟内存量 (Kbytes)
- **RSS**：该 process 占用的固定的内存量 (Kbytes)
- **TTY**：该 process 是在那个终端机上面运作，若与终端机无关，则显示 ?，tty1-tty6 是本机上面的登入者程序，若为 pts/0 等等的，则表示为由网络连接进主机的程序。
- **STAT**：进程状态
- **START**：进程启动时间
- **TIME**：该 process 实际使用 CPU 运作的时间

### 以类似进程树的方式显示

```shell
[root@localhost ~]# ps -auxf
root        898  0.0  0.0 115940   656 ?        Ss   4月12   0:00 /usr/bin/rhsmcertd
mysql      1205  0.7 10.0 1380688 389720 ?      Ssl  4月12   2:35 /usr/sbin/mysqld
root       1206  0.0  0.0 107976   592 ?        Ss   4月12   0:00 rhnsd
root       1246  0.0  0.0  82672  1272 ?        Ss   4月12   0:00 /usr/sbin/sshd
root       2201  0.0  0.1 145264  5212 ?        Ss   4月12   0:01  \_ sshd: root@pts/0
root       2203  0.0  0.0 116952  3600 pts/0    Ss   4月12   0:00  |   \_ -bash
root       4256  0.0  0.0  43580  2048 pts/0    T    00:15   0:00  |       \_ nc -l 99
root       4276  0.0  0.0 151256  1988 pts/0    R+   00:24   0:00  |       \_ ps auxf
root       2307  0.0  0.1 145392  5220 ?        Ss   4月12   0:02  \_ sshd: root@pts/1
root       2309  0.0  0.0 116880  3556 pts/1    Ss+  4月12   0:00      \_ -bash
root       2044  0.0  0.0  92128  2356 ?        Ss   4月12   0:00 /usr/libexec/postfix/master -w
postfix    2054  0.0  0.1  92300  4152 ?        S    4月12   0:00  \_ qmgr -l -t unix -u
postfix    4171  0.0  0.1  92232  4128 ?        S    4月12   0:00  \_ pickup -l -t unix -u
```

```shell
[root@localhost ~]# ps -ejH
   PID   PGID    SID TTY          TIME CMD
   898    898    898 ?        00:00:00   rhsmcertd
  1205   1205   1205 ?        00:03:03   mysqld
  1206   1206   1206 ?        00:00:00   rhnsd
  1246   1246   1246 ?        00:00:00   sshd
  2307   2307   2307 ?        00:00:03     sshd
  2309   2309   2309 pts/1    00:00:01       bash
  4381   4381   2309 pts/1    00:00:00         ps
  2044   2044   2044 ?        00:00:00   master
  2054   2044   2044 ?        00:00:00     qmgr
  4171   2044   2044 ?        00:00:00     pickup
```

### 显示线程有关信息

```shell
[root@localhost ~]# ps -eLf
UID         PID   PPID    LWP  C NLWP STIME TTY          TIME CMD
chrony      610      1    610  0    1 4月12 ?       00:00:00 /usr/sbin/chronyd
root        625      1    625  0    3 4月12 ?       00:00:00 /usr/sbin/NetworkManager --no-daemon
root        625      1    662  0    3 4月12 ?       00:00:00 /usr/sbin/NetworkManager --no-daemon
root        625      1    664  0    3 4月12 ?       00:00:00 /usr/sbin/NetworkManager --no-daemon
root        633      1    633  0    1 4月12 ?       00:00:00 /usr/sbin/crond -n
root        636      1    636  0    1 4月12 ?       00:00:00 /usr/sbin/atd -f
root        639      1    639  0    1 4月12 tty1    00:00:00 /sbin/agetty --noclear tty1 linux
root        885      1    885  0    3 4月12 ?       00:00:00 /usr/sbin/rsyslogd -n
root        885      1   1134  0    3 4月12 ?       00:00:00 /usr/sbin/rsyslogd -n
root        885      1   1135  0    3 4月12 ?       00:00:00 /usr/sbin/rsyslogd -n
root        888      1    888  0    5 4月12 ?       00:00:00 /usr/bin/python -Es /usr/sbin/tuned -l -P
root        888      1   2067  0    5 4月12 ?       00:00:00 /usr/bin/python -Es /usr/sbin/tuned -l -P
root        888      1   2068  0    5 4月12 ?       00:00:00 /usr/bin/python -Es /usr/sbin/tuned -l -P
root        888      1   2069  0    5 4月12 ?       00:00:04 /usr/bin/python -Es /usr/sbin/tuned -l -P
root        888      1   2070  0    5 4月12 ?       00:00:00 /usr/bin/python -Es /usr/sbin/tuned -l -P
root        898      1    898  0    1 4月12 ?       00:00:00 /usr/bin/rhsmcertd
mysql      1205      1   1205  0   38 4月12 ?       00:00:03 /usr/sbin/mysqld
mysql      1205      1   2146  0   38 4月12 ?       00:00:00 /usr/sbin/mysqld
mysql      1205      1   2147  0   38 4月12 ?       00:00:00 /usr/sbin/mysqld

```

### 仅显示PID为1的名称

```shell
[root@localhost ~]# ps -q 1 -o comm=
systemd
```

