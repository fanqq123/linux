# cachetop

显示Linux页面高速缓存命中/未命中统计信息，包括每个读和写命中百分比

## 格式

```shell
usage: cachetop [-h] [interval]
```

## 选项

```shell
-h	--help		帮助
```

## 实例

```shell
07:23:18 Buffers MB: 2 / Cached MB: 775 / Sort: HITS / Order: ascending
PID      UID      CMD              HITS     MISSES   DIRTIES  READ_HIT%  WRITE_HIT%
    1170 root     pool                    1        0        0     100.0%       0.0%
    2979 root     python                  1        0        0     100.0%       0.0%
    2982 root     pool                    1        0        0     100.0%       0.0%

```

- **MISSES** ：表示缓存未命中的次数
- **HITS** ：表示缓存命中的次数
- **DIRTIES**： 表示新增到缓存中的脏页数
- **READ_HIT**：读的缓存的命中率
- **WRITE_HIT**：写的缓存命中率