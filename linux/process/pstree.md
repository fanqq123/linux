# pstree

## 描述

将正在运行的进程显示为树

## 语法格式

```
用法: pstree [ -a ] [ -c ] [ -h | -H PID ] [ -l ] [ -n ] [ -p ] [ -g ] [ -u ]
              [ -A | -G | -U ] [ PID | USER ]
     pstree -V
```

## 常用选项

```shell
-a		显示命令行参数
-A		使用ASCII字符绘制树
-c		禁用相同子树的压缩。默认情况下，子树会尽可能压缩
-G		使用VT100线条图字符
-h		突出显示当前进程及其祖先
-H {PID}		与-h类似，但突出显示指定的进程
-g		显示PGID
-l		显示长行
-n		按PID而不是按名称对具有相同祖先的进程进行排序
-N		为指定类型的每个名称空间显示单独的树
-p		显示PID，可查询指定PID
-s		显示指定进程的父进程
-S		显示名称空间过渡。与-N一样，以常规用户身份运行时，输出受到限制
-u		显示uid转换。每当进程的uid与它的父进程的uid不同时，新uid就会在进程名称后的括号中显示
-U		使用UTF-8（Unicode）线描字符
-V		显示版本信息
-Z		（SELinux）显示每个进程的安全上下文。仅当pstree与SELinux支持兼容时，此标志才有效
```

## 实例

### 树状显示所以进程/线程信息并且显示其PID

```shell
[root@localhost ~]# pstree -p
systemd(1)─┬─NetworkManager(625)─┬─{NetworkManager}(662)
           │                     └─{NetworkManager}(664)
           ├─abrt-watch-log(598)
           ├─abrtd(596)
           ├─agetty(639)
           ├─atd(636)
           ├─auditd(565)───{auditd}(577)
           ├─chronyd(610)
           ├─crond(633)
           ├─dbus-daemon(606)
           ├─lsmd(592)
           ├─lvmetad(461)
           ├─master(2044)─┬─pickup(4432)
           │              └─qmgr(2054)
           ├─polkitd(602)─┬─{polkitd}(622)
           │              ├─{polkitd}(635)
           │              ├─{polkitd}(658)
           │              ├─{polkitd}(659)
           │              └─{polkitd}(660)
           ├─rhnsd(1206)
           ├─rhsmcertd(898)
           ├─rngd(599)
           ├─rsyslogd(885)─┬─{rsyslogd}(1134)
           │               └─{rsyslogd}(1135)
           ├─smartd(595)
           ├─sshd(1246)───sshd(2307)───bash(2309)───pstree(4481)
           ├─systemd-journal(444)
           ├─systemd-logind(603)
           ├─systemd-udevd(469)
           ├─tuned(888)─┬─{tuned}(2067)
           │            ├─{tuned}(2068)
           │            ├─{tuned}(2069)
           │            └─{tuned}(2070)
           └─vmtoolsd(590)───{vmtoolsd}(637)
```

