# cachestat 

计算缓存内核函数调用

## 格式

```
usage: cachestat [-h] [-T] [interval] [count]
```

## 选项

```shell
-h --help		显示此帮助消息并退出
-T --timestamp	在输出中包括时间戳
```

## 实例

```shell
[root@localhost ~]# cachestat 1 3
   TOTAL   MISSES     HITS  DIRTIES   BUFFERS_MB  CACHED_MB
       0        0        0        0            2        772
       0        0        0        0            2        772
       0        0        0        0            2        772
```

- **TOTAL** ：表示总的 I/O 次数
- **MISSES** ：表示缓存未命中的次数
- **HITS** ：表示缓存命中的次数
- **DIRTIES**： 表示新增到缓存中的脏页数
- **BUFFERS_MB**： 表示 Buffers 的大小，以 MB 为单位
- **CACHED_MB** ：表示 Cache 的大小，以 MB 为单位