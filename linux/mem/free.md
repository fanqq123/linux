# free

## 描述

显示系统中可用和可用的物理内存和交换内存的总量，以及内核使用的缓冲区和高速缓存。该信息由解析器收集`/proc/meminfo`

## 语法格式

```shell
用法:free [options]
```

## 常用参数

```shell
-b	--bytes	以字节为单位显示内存量
-k	--kilo	显示内存量（以kb为单位），默认值
-m	--mega	显示内存量（以MB为单位）
-g	--giga	显示内存量（以GB为单位）
-h	--human	显示所有输出字段自动缩放到最短的三位数单位并显示打印输出的单位
-w	--wide	切换到宽屏模式。宽屏模式产生的行长超过80个字符。
			在这种模式下，缓冲区和缓存在两个单独的列中报告
-c	--count	计数显示结果计数时间。需要-s选项
-l	-lohi	显示详细的低内存和高内存统计信息
-s	-seconds间隔连续显示结果延迟秒数。您实际上可以为延迟指定任何浮点数，
			usleep(3)用于微秒分辨率延迟次
--si		使用1000的幂而不是1024
-t	--total	显示一行以显示列总计
--help		打印帮助
-V	--version	显示版本信息
```

## 实例

```shell
[root@localhost ~]# free 
              total        used        free      shared  buff/cache   available
Mem:        3865524      591744     1888104       11912     1385676     2970948
Swap:       4064252           0     4064252
```

- **total**：总内存量（`/proc/meminfo`中的`MemTotal`和`SwapTotal`）
- **used**：已使用内存（ total - free - buffers - cache）
- **free**：空闲的未使用内存（`/proc/meminfo`中的`MemFree`和`SwapFree`）
- **shared**：共享内存（主要）由tmpfs使用
- **buff/cache**：内核缓冲区使用的内存（`Buffers in /proc/meminfo`）
- **available**：可用内存，预估有多少内存可用于启动新应用程序而无需交换,`available` 不仅包含未使用内存，还包括了可回收的缓存，所以一般会比未使用内存更大，并不是所有缓存都可以回收，因为有些缓存可能正在使用中