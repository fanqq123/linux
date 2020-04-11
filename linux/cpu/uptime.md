# uptime

## 描述

正常运行时间以一行显示以下信息。当前时间，系统已运行多长时间，当前已登录多少用户以及系统负载过去1、5和15分钟的平均值

## 格式

```shell
uptime [options]
```

## 选项

```shell
-p，--pretty   以漂亮的格式显示正常运行时间
-h，--help     显示此帮助文本
-s，--since    系统启动以来，格式为yyyy-mm-dd HH：MM：SS
-V，--version  显示版本信息并退出
```

## 实例

```shell
[fanqq@fanqq~]$ uptime 
09:48:07 up 39 days, 20:21,  2 users, load average: 0.01, 0.02, 0.05
 
09:48:07            #当前时间
up 39 days, 20:21   #系统运行时间
2 users             #正在登录用户数
0.01, 0.02, 0.05    #表示cpu过去1分钟、5分钟、15分钟的平均负载
平均负载与cpu使用率
```

