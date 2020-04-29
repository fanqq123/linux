# lsof

列出打开的文件，打开文件可以是常规文件，目录，块特殊文件，字符特殊文件，正在执行的文本引用，库，流或网络文件（Internet套接字，NFS文件或UNIX域套接字），特定文件或其中的所有文件可以通过路径选择文件系统。

## 格式

```shell
 用法: [-?abhKlnNoOPRtUvVX] [+|-c c] [+|-d s] [+D D] [+|-f[gG]] [+|-e s]
 [-F [f]] [-g [s]] [-i [i]] [+|-L [l]] [+m [m]] [+|-M] [-o [o]] [-p s]
[+|-r [t]] [-s [p:s]] [-S [t]] [-T [t]] [-u s] [+|-w] [-x [fl]] [--] [names]
```

## 选项
```bash
如果没有任何选项，lsof会列出属于所有活动进程的所有打开文件
-u 指定的登录用户名和用户ID
-U 选择UNIX域套接字文件列表
-p 指定的进程ID（PID）
-P 禁止将端口号转化为网络文件的端口名，可以使lsof运行快一点
-g 指的进程组ID（PGID）
-c 指定的command
	c以'^'开头，则以下字符指定将忽略其过程的命令名称
	c以斜杠（'/'）开头和结尾，则斜杠之间的字符将被解释为正则表达式
-a 可用于“与”操作
-s 指定的TCP/UDP协议状态名称
-i 选择其Internet地址与i中指定的地址相匹配的文件列表
-n 禁止将网络号转为网网络文件的主机名，可以使lsof运行快点
-N 选择NFS文件列表
-R 表示在PPID列中列出父进程ID标识号
-t 仅列出进程PID，可配合管道kill

```

## 输出值含义
```shell
COMMAND    包含与该进程关联的UNIX命令名称的前9个字符
PID        进程标识号
USER       是该进程所属用户的用户ID号或登录名
FD         是文件的文件描述符号
	cwd当前工作目录；
    Lnn库引用（AIX）；
    错误的FD信息错误（请参阅“名称”列）；
    jld监狱目录（FreeBSD）;
    ltx共享库文本（代码和数据）；
    Mxx十六进制内存映射类型xx。
    m86 DOS合并映射文件；
    mem内存映射文件；
    mmap内存映射设备；
    pd父目录；
    rtd根目录；
    tr内核跟踪文件（OpenBSD）;
    txt程序文本（代码和数据）；
    v86 VP / ix映射文件；
TYPE      是与文件关联的节点的类型，例如GDIR，GREG，VDIR，VREG等
DEVICE    包含设备号（用逗号分隔），用于特殊字符，特殊块，常规，目录或NFS文件
SIZE      SIZE/OFF或OFFSET是文件大小或文件偏移量（以字节为单位），仅当可用时才在此列中显示一个值，Lsof显示适合于文件类型和lsof版本的任何值-大小或偏移量
NODE      是本地文件的节点号
NAME      是文件所在的安装点和文件系统的名称
```

## 实例

### 根据端口号查找PID

```shell
lsof -i :9150
```
### 查找所有监听端口

```shell
lsof  -i -s TCP:LISTEN
```
### 列出fanqq用户打开的所有文件

```shell
lsof -u fanqq
```
### 列出那些进程在使用fanqq.txt文件

```shell
lsof fanqq.txt
```
### 列出PID为2222打开的所有文件

```shell
lsof -p 2222
```
### 恢复误删的error.log文件（文件调用的进程必须存在）

```shell
找到error.log文件调用的进程PID
cd /proc/PID/fd
ll|grep error.log找出指向error.log的链接文件
cat 链接文件> 写入原目录
```