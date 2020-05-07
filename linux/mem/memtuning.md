# 内存性能调优

## swap

### 调整swappiness值

```shell
临时修改：
sudo sysctl vm.swappiness=10

永久调整：
在/etc/sysctl.conf文件里添加 vm.swappiness=10这一行
```

### 关闭swap分区

```shell
临时修改:
sudo swapoff -a

永久修改：
删除掉/etc/fstab里的swap挂载
```

## buffer/cache

### 手动释放缓存

```elm
echo 1 > /proc/sys/vm/drop_caches

drop_caches的值可以是0-3之间的数字，代表不同的含义：
0：不释放（系统默认值）
1：释放页缓存
2：释放dentries(目录的数据结构)和inodes(文件数据结构)
3：释放所有缓存
```

## 调整核心程序oom_score

```elm
收到通过调整进程/proc/pid/oom_adj值来调整
oom_adj 的范围是 [-17, 15]，数值越大，表示进程越容易被 OOM 杀死；数值越小，表示进程越不容易被 OOM 杀死，其中 -17 表示禁止 OOM
实例：
echo -16 > /proc/$(pidof sshd)/oom_adj
```

