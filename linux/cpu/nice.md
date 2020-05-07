# nice

改变进程的优先级

## 格式

```elm
Usage: nice [OPTION] [COMMAND [ARG]...]
```

## 选项

```elm
-n	--adjustment=N		设置进程优先级的谦让值（修正值）为N，默认值为10
--help					显示帮助
--version				显示版本信息	
```

adjustment的值范围为-20到19

adjustment值越低表示进程优先级越高没，越低表示进程优先级越低

