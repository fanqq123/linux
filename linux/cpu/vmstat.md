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

![image-20200409183411930](E:\markdata\images\image-20200409183411930.png)

```erlang
procs进程：
	r：     可运行的进程数（正在运行或正在等待运行时）
	b：     等待IO进程数
memory内存：
	swpd：  使用的虚拟内存量
	free：  空闲内存量
	buff：  用作缓存区内存量
	cache： 用作缓存内存量
	inact:	不活跃内存量
	active	活跃内存量
swap交换空间：
	si：    从磁盘交换的内存量（内存从交换移至实际内存）
	so：    交换到磁盘的内存量（内存从实际内存移到交换）
io：(现在的Linux版本块的大小为1024bytes)
	bi：    每秒读取的块数
	bo：	   每秒写入的块数
system：
	in：	   每秒的中断数	
	cs：	   每秒上下文切换数	
cpu：
	us：    用户进程消耗的时间
	sy：    系统进程消耗的时间
	id：	   空闲
	wa：    等待IO所花费的时间
	st：    从虚拟机窃取的时间

注解：
如果r值长期超过逻辑cpu核数，且id小于40，表示cpu负荷很重
如果bi、bo长期不等于0表示内存不足

```

## 磁盘模式

![image-20200409183452010](E:\markdata\images\image-20200409183452010.png)

```erlang
reads：
	total：		总读取成功完成
	merged：		分组读取(导致一个IO)
	sectors：	成功读取扇区
	ms：			花费的毫秒数
writes：
	total：		总写入成功完成
	merged：		分组写入(导致一个IO)
	sectors：	成功写入扇区
	ms：			花费的毫秒数
IO：
	cur：		I/O正在进行中
	sec：		IO的花费的秒数
	
```

## 磁盘分区模式

![image-20200409183748423](E:\markdata\images\image-20200409183748423.png)

```erlang
reads：		此分区的读取总数
read sectors：	分区读取扇区总数
writes：		此分区的写入总数
requested writes：	分区写入扇区总数
```

## 磁盘总结信息

![image-20200409195859445](E:\markdata\images\image-20200409195859445.png)

## slab模式

![image-20200409183723632](E:\markdata\images\image-20200409183723632.png)

```erlang
cache：		缓存的名称
num：		当前活动对象的数量
total：		可用对象总数
size：		每个对象大小
pages：		具有至少一个活动对象的页面数
```

## 更多统计信息

![image-20200409201332885](E:\markdata\images\image-20200409201332885.png)

