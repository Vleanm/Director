CPU绑定

CPU热添加

```
virsh setvcpus cent 5 --live
echo 1 > /sys/devices/system/cpu/cpu4/online
```

CPU减少

```
CPU不支持热去除，可以在虚拟机中关闭CPU
echo 0 > /sys/devices/system/cpu/cpu4/online
```

KSM内存合并（对内存页进行整理合并，需要对内存进行扫描，会增加CPU负载）

内存气球（动态调整内存大小）