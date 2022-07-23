### linux

#### linux 下取进程占用 cpu/内存 最高的前10个进程

> linux 下 取进程占用 cpu 最高的前10个进程

```
ps aux|head -1;ps aux|grep -v PID|sort -rn -k +3|head
```

> linux 下 取进程占用内存(MEM)最高的前10个进程

```
ps aux|head -1;ps aux|grep -v PID|sort -rn -k +4|head
```