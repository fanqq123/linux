# proc/softirqs

## 描述

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

