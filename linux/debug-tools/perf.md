# perf

## 描述

Linux的性能分析工具

## 语法格式

```shell
用法: perf [--version] [--help] [OPTIONS] COMMAND [ARGS]
perf stat：此命令为常见性能事件提供整体数据，包括执行步骤和消耗所用的时间周期
perf record：此命令将性能数据记录到随后可使用 perf report 分析的文件中
perf report：此命令从文件中读取性能数据并分析记录数据
perf list：此命令列出特定机器上有效事件。这些事件因系统性能监控硬件和软件配置而异
perf top：此命令执行与 top 工具相似的功能。它实时生成并显示性能计数器配置文件
perf trace：此命令执行与 strace 工具相似的功能。它监控特定线程或进程使用的系统调用以及该应用程序接收的所有信号
```

## 实例

### perf stat

```shell
          1,709.77 msec cpu-clock                 #    1.000 CPUs utilized          
               535      context-switches          #    0.313 K/sec                  
                 0      cpu-migrations            #    0.000 K/sec                  
                 5      page-faults               #    0.003 K/sec                  
   <not supported>      cycles                                                      
   <not supported>      instructions                                                
   <not supported>      branches                                                    
   <not supported>      branch-misses                                               

       1.709779435 seconds time elapsed
```

- **msec cpu-clock**：CPU 利用率，该值高，说明程序的多数时间花费在 CPU 计算上而非 IO
- **context-switches**：进程切换次数，记录了程序运行过程中发生了多少次进程切换，频繁的进程切换是应该避免的
- **cpu-migrations**：表示进程 t1 运行过程中发生了多少次 CPU 迁移，即被调度器从一个 CPU 转移到另外一个 CPU 上运行
- **page-faults**：
- **cycles **：处理器时钟，一条机器指令可能需要多个 cycles
- **instructions**：机器指令数目
- 

