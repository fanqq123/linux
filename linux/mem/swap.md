# swap

## 调整swappiness值

```shell
临时修改：
sudo sysctl vm.swappiness=10

永久调整：
在/etc/sysctl.conf文件里添加 vm.swappiness=10这一行
```

## 关闭swap分区

```shell
临时修改:
sudo swapoff -a

永久修改：
删除掉/etc/fstab里的swap挂载
```