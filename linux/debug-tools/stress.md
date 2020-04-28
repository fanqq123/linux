# stress

## 描述

施加压力和压力测试系统的工具

## 语法格式

```shell
用法: stress [OPTION [ARG]] ...
```

## 常用选项

```shell
-?	--help	显示帮助声明
--version		查询版本信息
-c	--cpu N	产生N个进程，每个进程都反复不停的计算随机数平方根
-i	--io N	产生N个进程，每个进程反复调用sync()将内存上的内容写入到磁盘
-m	--vm N	产生N个进程，每个进程不断分配和释放内存
	--vm-bytes B    指定分配内存的大小
    --vm-stride B    不断的给部分内存赋值，让 COW(Copy On Write)发生
   	--vm-hang N     指示每个消耗内存的进程在分配到内存后转入睡眠状态 N 秒，然后释放内存，一直重复执行这个过程
    --vm-keep         一直占用内存，区别于不断的释放和重新分配(默认是不断释放并重新分配内存)
-d	--hadd N          产生 N 个不断执行 write 和 unlink 函数的进程(创建文件，写入内容，删除文件)
    --hadd-bytes B    指定文件大小
-t	--timeout N       在 N 秒后结束程序        
--backoff N           等待N微妙后开始运行
-q	--quiet           程序在运行的过程中不输出信息
-n	--dry-run         输出程序会做什么而并不实际执行相关的操作
```

## 实例

### 压测cpu

```shell
stress	-c	1	
```

### 压测mem

```

```

### 压测磁盘、io

```shell
stress -i 1 
```

