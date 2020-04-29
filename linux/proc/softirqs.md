# proc/softirqs

内核软中断信息

## 实例

```shell
[fanqq@fanqq~]$ cat /proc/softirqs 
                    CPU0       CPU1       
          HI:          1          0
       TIMER:  326777284  325092263
      NET_TX:        105         87
      NET_RX:    7641864     602329
       BLOCK:          0          0
BLOCK_IOPOLL:          0          0
     TASKLET:        428         10
       SCHED:  192385900  189871611
     HRTIMER:          0          0
         RCU:  151819643  148507847
```

- HI：最高优先级的软中断类型
- TIMER：timer定时器的软中断
- NET_TX: 发送网络数据包的软中断
- NET_RX: 接收网络数据包的软中断
- BLOCK: 快设备的软中断
- TASKLET： 专门为 tasklet 机制准备的软中断
- SCHED：进程调度以及负载均衡
- HRTIMER：高精度定时器
- RCU：专门为 RCU 服务的软中断