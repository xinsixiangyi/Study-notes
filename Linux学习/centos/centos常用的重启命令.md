# centos常用的重启命令有哪些？



为什么服务器不能直接断电？

服务器正在高速运转，如果不采用正确关机重启会导致硬件损坏。

### 一、shutdown

关闭所有服务过后才会关机

-c：取消前一个关机命令

-h：关机

-r：重启

-k：向所有用户发送关机提醒，不会关机

```shell
# 取消关机命令
shutdown -c
# 立即关机(now立即)
shutdown -h now
# 定时20:30关机
shutdown -h 20:30
# 定时10分钟后关机
shutdown -h +10
# 立即重启(now立即)
shutdown -r now
# 定时20:30重启
shutdown -r 20:30
#定时10分钟后重启
shutdown -r +10
```



### 二、其他

halt：直接进行硬件级关机

poweroff：关机

init 0：关机

reboot：重启

init 6：重启

系统运行级别

0 关机

1 单用户

2 不完全多用户，不含NFS服务

3 完全多用户

4 未分配

5 图形界面

6 重启