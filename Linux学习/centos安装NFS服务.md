NFS服务搭建与配置

NFS介绍

NFS是Network File System的缩写

NFS最早由Sun公司开发，分2,3,4三个版本，2和3由Sun起草开发，4.0开始Netapp公司参与并主导开发，最新为4.1版本

NFS数据传输基于RPC协议，RPC为Remote Procedure Call的简写。

NFS应用场景是：A,B,C三台机器上需要保证被访问到的文件是一样的，A共享数据出来，B和C分别去挂载A共享的数据目录，从而B和C访问到的数据和A上的一致

总结：NFC服务需要借助RPC协议实现通信。

**NFS服务端安装配置**

实验需要2台机器，一台作为服务端，一台作为客户端。

服务端，安装2个包nfs-utils和rpcbind

 

```shell
 [root@zyshanlinux-001 ~]# yum install -y nfs-utils rpcbind   
```

 Installed:    nfs-utils.x86_64 1:1.3.0-0.54.el7  rpcbind.x86_64 0:0.2.0-44.el7 

客户端，安装包nfs-utils

 

```shell
[root@zyshanlinux-02 ~]# yum install -y nfs-utils
```

Installed:    nfs-utils.x86_64 1:1.3.0-0.54.el7

配置文件，允许共享主机IP

```shell
[root@zyshanlinux-001 ~]# vim /etc/exports
```

配置内容，就一行

```shell
/home/nfstestdir 192.168.106.0/24(rw,sync,all_squash,anonuid=1000,anongid=1000)
```

保存配置文件后，执行如下准备操作

首先要创建分享的目录，给创建的目录赋予777的权限。

```shell
[root@zyshanlinux-001 ~]# mkdir /home/nfstestdir  

[root@zyshanlinux-001 ~]# chmod 777 /home/nfstestdir
```

服务端启动rpcbind前后监听端口情况

```shell
[root@zyshanlinux-001 ~]# netstat -lnpt
  Active Internet connections (only servers)
  Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
  tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1254/nginx: master  
  tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1086/sshd           
  tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1325/master         
  tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      1254/nginx: master  
  tcp6       0      0 :::22                   :::*                    LISTEN      1086/sshd           
  tcp6       0      0 ::1:25                  :::*                    LISTEN      1325/master         
  tcp6       0      0 :::3306                 :::*                    LISTEN      1447/mysqld         
[root@zyshanlinux-001 ~]# systemctl start rpcbind
[root@zyshanlinux-001 ~]# netstat -lnpt
  Active Internet connections (only servers)
  Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
  tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      46926/rpcbind       
  tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1254/nginx: master  
  tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1086/sshd           
  tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1325/master         
  tcp        0      0 0.0.0.0:443             0.0.0.0:*               LISTEN      1254/nginx: master  
  tcp6       0      0 :::111                  :::*                    LISTEN      46926/rpcbind       
  tcp6       0      0 :::22                   :::*                    LISTEN      1086/sshd           
  tcp6       0      0 ::1:25                  :::*                    LISTEN      1325/master         
  tcp6       0      0 :::3306                 :::*                    LISTEN      1447/mysqld 

```

客户端启动rpcbind前后监听端口情况

  

```shell
  [root@zyshanlinux-02 ~]# netstat -lntp
  Active Internet connections (only servers)
  Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
  tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      878/sshd            
  tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1113/master         
  tcp6       0      0 :::22                   :::*                    LISTEN      878/sshd            
  tcp6       0      0 ::1:25                  :::*                    LISTEN      1113/master         
  [root@zyshanlinux-02 ~]# ps aux |grep rpc
  root     21597  0.0  0.0 112660   964 pts/0    R+   20:38   0:00 grep --color=auto rpc
  [root@zyshanlinux-02 ~]# systemctl start rpcbind
  [root@zyshanlinux-02 ~]# netstat -lntp
  Active Internet connections (only servers)
  Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
  tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      22898/rpcbind       
  tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      878/sshd            
  tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      1113/master         
  tcp6       0      0 :::111                  :::*                    LISTEN      22898/rpcbind       
  tcp6       0      0 :::22                   :::*                    LISTEN      878/sshd            
  tcp6       0      0 ::1:25                  :::*                    LISTEN      1113/master         
  [root@zyshanlinux-02 ~]# ps aux |grep rpc
  rpc      22898  0.0  0.0  64956  1048 ?        Ss   20:39   0:00 /sbin/rpcbind -w
  root     23120  0.0  0.0 112660   968 pts/0    R+   20:39   0:00 grep --color=auto rpc
```

启动NFS

```shell
  [root@zyshanlinux-001 ~]# systemctl start nfs
  [root@zyshanlinux-001 ~]# ps aux |grep nfs
  root     51317  0.0  0.0      0     0 ?        S<   20:42   0:00 [nfsd4_callbacks]
  root     51323  0.0  0.0      0     0 ?        S    20:42   0:00 [nfsd]
  root     51324  0.0  0.0      0     0 ?        S    20:42   0:00 [nfsd]
  root     51325  0.0  0.0      0     0 ?        S    20:42   0:00 [nfsd]
  root     51326  0.0  0.0      0     0 ?        S    20:42   0:00 [nfsd]
  root     51327  0.0  0.0      0     0 ?        S    20:42   0:00 [nfsd]
  root     51328  0.0  0.0      0     0 ?        S    20:42   0:00 [nfsd]
  root     51329  0.0  0.0      0     0 ?        S    20:42   0:00 [nfsd]
  root     51330  0.0  0.0      0     0 ?        S    20:42   0:00 [nfsd]
  root     51574  0.0  0.0 112704   960 pts/0    R+   20:42   0:00 grep --color=auto nfs
  [root@zyshanlinux-001 ~]# ps aux |grep rpc
  rpc      46926  0.0  0.0  69220  1428 ?        Ss   20:38   0:00 /sbin/rpcbind -w
  root     51289  0.0  0.0      0     0 ?        S<   20:42   0:00 [rpciod]
  rpcuser  51290  0.0  0.0  42420  1752 ?        Ss   20:42   0:00 /usr/sbin/rpc.statd
  root     51307  0.0  0.0  42608   940 ?        Ss   20:42   0:00 /usr/sbin/rpc.mountd
  root     51308  0.0  0.0  45924   540 ?        Ss   20:42   0:00 /usr/sbin/rpc.idmapd
  root     52778  0.0  0.0 112704   956 pts/0    R+   20:43   0:00 grep --color=auto rpc
```

服务端开机启动NFS

```shell
  [root@zyshanlinux-001 ~]# systemctl enable nfs
  Created symlink from /etc/systemd/system/multi-user.target.wants/nfs-server.service to /usr/lib/systemd/system/nfs-server.service.
```

**NFS配置选项**

```shell
  [root@zyshanlinux-001 ~]# cat /etc/exports  /home/nfstestdir 
```

192.168.106.0/24(rw,sync,all_squash,anonuid=1000,anongid=1000)

rw 读写 ro 只读 sync 同步模式，内存数据实时写入磁盘 async 非同步模式 no_root_squash 客户端挂载NFS共享目录后，root用户不受约束，权限很大 root_squash 与上面选项相对，客户端上的root用户收到约束，被限定成某个普通用户 all_squash 客户端上所有用户在使用NFS共享目录时都被限定为一个普通用户 anonuid/anongid 和上面几个选项搭配使用，定义被限定用户的uid和gid

\##客户端连接服务端的IP

```shell
[root@zyshanlinux-02 ~]# showmount -e 192.168.106.128  

clnt_create: RPC: Port mapper failure - Unable to receive: errno 113 (No route to host)
```

报错，排除是网络不通的问题，那就需要关闭防火墙，服务端客户端都关闭。

服务端：

1、关闭防火墙 2、关闭SELinux，但服务端已经关闭。

```shell
[root@zyshanlinux-001 ~]# systemctl stop firewalld  

[root@zyshanlinux-001 ~]# setenforce 0  
```

setenforce: SELinux is disabled

客户端：

1、关闭防火墙 2、关闭SELinux

[root@zyshanlinux-02 ~]# systemctl stop firewalld 

[root@zyshanlinux-02 ~]# getenforce  Enforcing 

[root@zyshanlinux-02 ~]# setenforce 0



连接服务端IP成功

[root@zyshanlinux-02 ~]# showmount -e 192.168.106.128  

Export list for 192.168.106.128:  /home/nfstestdir 192.168.106.0/24

挂载服务端共享的目录，用df -h测试，挂载成功

[root@zyshanlinux-02 ~]# mount -t nfs 192.168.106.128:/home/nfstestdir /mnt  [root@zyshanlinux-02 ~]# df -h  Filesystem             

```shell
  Filesystem                        Size  Used Avail Use% Mounted on
  /dev/sda3                          28G 1011M   27G   4% /
  devtmpfs                          907M     0  907M   0% /dev
  tmpfs                             916M     0  916M   0% /dev/shm
  tmpfs                             916M  8.8M  908M   1% /run
  tmpfs                             916M     0  916M   0% /sys/fs/cgroup
  /dev/sda1                         197M  113M   85M  58% /boot
  tmpfs                             184M     0  184M   0% /run/user/0
  192.168.106.128:/home/nfstestdir   28G  7.3G   21G  27% /mnt
```



由于配置文件上设置了属主和属组，可以看到文件的属主和属组都为1000

客户端：创建文件，创建的文件的属主和属组都为1000，由于没有该用户，都用1000代替

```shell
  [root@zyshanlinux-02 mnt]# touch zyshanlinux.111
  [root@zyshanlinux-02 mnt]# ls -l
  total 0
  -rw-r--r--. 1 1000 1000 0 Jul 15 21:20 zyshanlinux.111
  [root@zyshanlinux-02 mnt]# id 1000
  id: 1000: no such user
```



服务端：客户端创建的文件在服务端查看，属主是user1，属组是1000

```shell
  [root@zyshanlinux-001 ~]# ls -l /home/nfstestdir
  total 0
  -rw-r--r-- 1 user1 1000 0 Jul 15 21:20 zyshanlinux.111
  [root@zyshanlinux-001 ~]# id user1
  uid=1000(user1) gid=1001(user1) groups=1001(user1)
```

**exportfs命令**

如果要服务端要关闭或重启NFS，需要先把客户端挂载服务端的目录先卸载

```shell
  [root@zyshanlinux-02 mnt]# umount /mnt
  umount.nfs4: /mnt: device is busy
  [root@zyshanlinux-02 mnt]# cd 
  [root@zyshanlinux-02 ~]# umount /mnt
  [root@zyshanlinux-02 ~]# 
```

由于服务端不能随意关闭或重启nfs，会导致客户端正在挂载的目录读写会出现问题。引入了exportfs命令，不许重启NFS服务，配置文件也会生效。

修改配置文件

  [root@zyshanlinux-001 ~]# !vi  vim /etc/exports

新增配置内容，记得IP是允许的客户端IP

  /home/nfstestdir 192.168.106.0/24(rw,sync,all_squash,anonuid=1000,anongid=1000)  /tmp 192.168.106.130(rw,sync,no_root_squash)

服务端，全部共享目录重新挂载并显示，不用重启nfs服务，配置文件就会生效

```shell
  [root@zyshanlinux-001 ~]# exportfs -arv
  exporting 192.168.106.130:/tmp
  exporting 192.168.106.0/24:/home/nfstestdir
```

客户端也生效了。

```shell
  [root@zyshanlinux-02 ~]# !showm
  showmount -e 192.168.106.128
  Export list for 192.168.106.128:
  /home/nfstestdir 192.168.106.0/24
  /tmp             192.168.106.130
```

客户端挂载

 

```
  [root@zyshanlinux-02 ~]# mount -t nfs 192.168.106.128:/tmp/ /mnt/
  [root@zyshanlinux-02 ~]# cd /mnt
  [root@zyshanlinux-02 mnt]# ls
  mysql2.sql               systemd-private-d14aa6709ba64c9ca559b305bd7b0b86-chronyd.service-Tpa4NU
  mysql_all.sql            systemd-private-d14aa6709ba64c9ca559b305bd7b0b86-vgauthd.service-iHfxUd
  mysql.sock               systemd-private-d14aa6709ba64c9ca559b305bd7b0b86-vmtoolsd.service-mSuqtC
  mysql.sql                test.com.log
  pear                     test.com.log-20180704
  php_errors.log-20180704  user.sql
  php-fcgi.sock
```

由于配置文件中写了no_root_squash，所以客户端和服务端属主属组的差异

```shell
  [root@zyshanlinux-02 mnt]# vi 1212.txt
  [root@zyshanlinux-02 mnt]# ls -l 1212.txt
  -rw-r--r--. 1 root root 28 Jul 15 22:21 1212.txt
  ​
  [root@zyshanlinux-001 tmp]# ls -l 1212.txt
  -rw-r--r-- 1 root root 28 Jul 15 22:21 1212.txt
```

**NFS客户端问题**

NFS 4版本会有该问题 客户端挂载共享目录后，不管是root用户还是普通用户，创建新文件时属主、属组为nobody 客户端挂载时加上 -o nfsvers=3 客户端和服务端都需要 vim /etc/idmapd.conf // 把“#Domain = local.domain.edu” 改为 “Domain = xxx.com” （这里的xxx.com,随意定义吧），然后再重启rpcidmapd服务

先在挂载前

mount -t nfs 192.168.106.128:/tmp/ /mnt/

再挂载

mount -t nfs -oremount,nfsvers=3 192.168.106.128:/tmp/ /mnt/

```shell
  [root@zyshanlinux-02 ~]# mount -t nfs -oremount,nfsvers=3 192.168.106.128:/tmp/ /mnt/
  mount.nfs: an incorrect mount option was specified
  [root@zyshanlinux-02 ~]# cd
  [root@zyshanlinux-02 ~]# umount /mnt/
  umount: /mnt/: not mounted
  [root@zyshanlinux-02 ~]# mount -t nfs -oremount,nfsvers=3 192.168.106.128:/tmp/ /mnt/
  mount.nfs: an incorrect mount option was specified
  [root@zyshanlinux-02 ~]# mount -t nfs -o nfsvers=3 192.168.106.128:/tmp/ /mnt/
  [root@zyshanlinux-02 ~]# mount -t nfs -oremount,nfsvers=3 192.168.106.128:/tmp/ /mnt/
  [root@zyshanlinux-02 ~]# df -h
  Filesystem             Size  Used Avail Use% Mounted on
  /dev/sda3               28G  1.1G   27G   4% /
  devtmpfs               907M     0  907M   0% /dev
  tmpfs                  916M     0  916M   0% /dev/shm
  tmpfs                  916M  8.7M  908M   1% /run
  tmpfs                  916M     0  916M   0% /sys/fs/cgroup
  /dev/sda1              197M  113M   85M  58% /boot
  tmpfs                  184M     0  184M   0% /run/user/0
  192.168.106.128:/tmp/   28G  7.3G   21G  27% /mnt
  [root@zyshanlinux-02 ~]# mount -t nfs -oremount,nfsvers=3 192.168.106.128:/tmp/ /mnt/
```

