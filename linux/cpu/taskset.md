# taskset

tasket用于设置或检索给定PID的运行进程的CPU亲和力，或使用给定的CPU亲和力启动新的COMMAND。

CPU关联性是一个调度程序属性，可将进程“绑定”到系统上一组给定的CPU。

Linux调度程序将遵循给定的CPU关联性，并且该进程将不会在任何其他CPU上运行。

## 格式

```shell
用法: taskset [options] [mask | cpu-list] [pid|cmd [args...]]
```

## 参数

```shell
-a --all-tasks		设置或获取给定PID的所有任务（线程）的CPU关联性
-p --pid			在现有的PID上操作，不启动新任务
-c，--cpu-list	    指定处理器的数字列表，而不是位掩码。这些数字用逗号分隔，并且可以包括范围。例如：						 0,5,7,9-11
-h，--help			显示用法信息并退出
-V，--version		显示版本信息并退出
```

## 实例

### 查询线程可用cpu

```elm
[root@fanqq fanqq]# taskset -pc 1
pid 1's current affinity list: 0,1
```

> 上述命令的结果表示PID为1的进程可以被安排在0和1这两个cpu上运行

### 设置线程的可用cpu

```elm
[root@fanqq fanqq]# taskset -pc 0 1
pid 1's current affinity list: 0,1
pid 1's new affinity list: 0
```

### 查线程运行在那个cpu上

```elm
[root@localhost mybook]# ps -o pid,psr -p 1
   PID PSR
     1   0
```

