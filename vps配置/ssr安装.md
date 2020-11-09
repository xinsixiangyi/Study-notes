## 安装ss(r):

```shell
yum -y update
wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```

### 选择安装

![img](D:\work\notbook\xinsixiangyi7@163.com\42a1de2f088f47c5bcc4e7f8724f210e\clipboard.png)

以后执行

```shell
 chmod +x ssr.sh && bash ssr.sh
```

### 安装锐速

```shell
#如果失败切换内核

wget —no–check–certificate https://file.kskxs.com/linux/kernel/ruisu.sh&&chmod +x ssr.sh && bash ruisu.sh bash ruisu.sh

#查看系统内核 
uname -r 
```

### 谷歌BBR

我的环境：CentOS

1、下载、授权、执行安装 

1. ```shell
   $ wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh
   $ chmod +x bbr.sh
   $ bash bbr.sh
   ```

   

2、安装好后重启linux

1. $ reboot

3、检验

1. $ sysctl net.ipv4.tcp_available_congestion_control

解释：返回 net.ipv4.tcp_available_congestion_control = cubic reno bbr 说明BBR已完成安装。

1. $ lsmod | grep bbr

解释：返回值有 tcp_bbr 说明BBR已启动。

CentOS7安装新版内核和开启BBR加速教程_BBR2一键包

BBR是Google开源的一种TCP网络拥塞优化算法，可以加速访客到你服务器的访问速度。尤其是国外服务器，开启bbr算法会对[网站优化](https://blog.naibabiji.com/series/wordpress-optimization)有一定的帮助。

这里奶爸就使用CentOS7系统给大家演示一下如何安装新版的内核（因为BBR要内核4.9以上）并且开启BBR加速。

https://blog.naibabiji.com/skill/centos7-an-zhuang-bbr.html#bbr_plus_yi_jian_bao)

## CentOS7安装新版内核的步骤

首先是查看当前服务器的内核版本。

uname -r

uname命令用于打印当前系统相关信息（内核版本号、硬件架构、主机名称和操作系统类型等）。

-a或--all：显示全部的信息； -m或--machine：显示电脑类型； -n或-nodename：显示在网络上的主机名称； -r或--release：显示操作系统的发行编号； -s或--sysname：显示操作系统名称； -v：显示操作系统的版本； -p或--processor：输出处理器类型或"unknown"； -i或--hardware-platform：输出硬件平台或"unknown"； -o或--operating-system：输出操作系统名称； --help：显示帮助； --version：显示版本信息。

BBR内核要求是4.9+，通常来说你通过上面这个命令出来的内核版本是在3.几。

接下来启用 ELRepo 仓库

rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org rpm -Uvh https://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm

![img](D:\work\notbook\xinsixiangyi7@163.com\fb0d4b94d5a1474a97a13e878c442cf2\-24_01-11-15.png)

然后安装新版的稳定版内核

yum --enablerepo=elrepo-kernel install kernel-ml -y

安装完毕后使用下面的命令查看是否安装成功。

rpm -qa | grep kernel

我的显示如下：

kernel-3.10.0-862.14.4.el7.x86_64 kernel-ml-5.3.8-1.el7.elrepo.x86_64 kernel-3.10.0-1062.4.1.el7.x86_64 kernel-headers-3.10.0-1062.4.1.el7.x86_64 kernel-3.10.0-957.5.1.el7.x86_64 kernel-3.10.0-1062.1.2.el7.x86_64 kernel-tools-3.10.0-1062.4.1.el7.x86_64 kernel-tools-libs-3.10.0-1062.4.1.el7.x86_64 kernel-3.10.0-957.1.3.el7.x86_64

里面kernel-ml-5.3.8-1.el7.elrepo.x86_64就是安装的新版版本内核（你看到这篇教程的时候可能内核版本有变化，随机应变）

接下来需要设置系统启动顺序，使用下面的命令。

sudo egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'

我的显示如下：

CentOS Linux (5.3.8-1.el7.elrepo.x86_64) 7 (Core) CentOS Linux (3.10.0-1062.4.1.el7.x86_64) 7 (Core) CentOS Linux (3.10.0-1062.1.2.el7.x86_64) 7 (Core) CentOS Linux (3.10.0-957.5.1.el7.x86_64) 7 (Core) CentOS Linux (3.10.0-957.1.3.el7.x86_64) 7 (Core) CentOS Linux (3.10.0-862.14.4.el7.x86_64) 7 (Core) CentOS Linux (0-rescue-618ca2de6e204efbb013b592564ef36a) 7 (Core)

排在第一的就是CentOS Linux (5.3.8-1.el7.elrepo.x86_64) 7 (Core)，从第一行为0依次数，0、1、2、3这样，看你的新内核是第几。

然后就输入下面的命令（命令例子为第1行）

sudo grub2-set-default 0

接下来重启服务器

reboot

再次查看内核版本

uname -r

内核版本显示为4.9以上，本文更新的时候新版版本是5.3.8，就证明安装成功了。

重建内核配置

grub2-mkconfig -o /boot/grub2/grub.cfg

重启系统验证，没问题就OK了。

 

在CentOS7新内核上开启BBR

要在新安装好的CentOS7上面启用新内核，只需要复制下面的代码执行就可以了。

echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf sudo sysctl -p

然后输入下面的命令查看是否开启BBR成功

sudo sysctl net.ipv4.tcp_available_congestion_control

成功的话应该是下面这种输出

net.ipv4.tcp_available_congestion_control = bbr cubic reno

继续验证

sudo sysctl -n net.ipv4.tcp_congestion_control

输出应该是

bbr

最后看内核模块是否加载

lsmod | grep bbr

输出应该是类似下面这种

tcp_bbr 16384 0

开启BBR有什么用？

简单来说，开启BBR可以对你网站访问速度起到一定的优化。例如奶爸的笔记使用的是WordPress，通过BBR也可以给WordPress网站进行一定的加速优化，当然，奶爸采用的国内服务器，所以BBR加速效果也不会有多明显。

BBR是Google开源的一种TCP网络拥塞优化算法，TCP BBR 致力于解决两个问题：在有一定丢包率的网络链路上充分利用带宽。降低网络链路上的 buffer 占用率，从而降低延迟。TCP 拥塞控制的目标是最大化利用网络上瓶颈链路的带宽。

开源地址：https://github.com/google/bbr

BBR和BBR2一键包

什么是BBR2？

BBR2目前还是预览版，是BBR的升级版本，目前还不够成熟，不建议生产环境使用。

BBR2详细说明参见：https://github.com/google/bbr/blob/v2alpha/README.md

同时说明文件里面包含手动安装bbr2的教程和步骤，这里就不复制了。

bbr2一键包

警告：更换内核有风险，若使用本脚本后无法开机造成损失，概不负责。

建议系统 Debian 10 x86_64，理论支持Debian 8+, Ubuntu 16.04+

只适用于KVM虚拟架构VPS，如果是OVZ、Xen、或者独服就别试了。

仅适用于64位(x86_64)系统，不支持x86，**不支持CentOS及其他系统**。

已在搬瓦工 Debian 8 9 10 , Ubuntu 16.04 18.04 中测试通过 (Ubuntu 14.04 失败)

已在以下商家的Debian 10系统中测试通过：Oracle Public Cloud, DMIT, OLVPS, AlibabaCloud

Debian 10 安装成功率100%

CentOS系统用户建议使用bbr或者尝试手动安装bbr2。

wget --no-check-certificate -q -O bbr2.sh "https://raw.githubusercontent.com/yeyingorg/bbr2.sh/master/bbr2.sh" && chmod +x bbr2.sh && bash bbr2.sh auto

重要！请开/关BBR/ECN后重启一下系统。发现脚本可能有BUG导致不生效或者跟旧的bbr等加速共存。

BBR一键包

bbr一键包支持的系统多一些，不过前提是内核版本要支持bbr才行，如果内核版本不行还是需要自己手动更新内核。

系统支持：CentOS 6+，Debian 7+，Ubuntu 12+

虚拟技术：OpenVZ 以外的，比如 KVM、Xen、VMware 等

内存要求：≥128M

wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh

bbr plus一键包

bbr plus就是在原版bbr的基础上进行了参数优化，在某些机器上加速效果比原版的更好。

本文介绍的BBR Plus一键安装脚本，来自网友cx9208。除了BBR Plus外，还另外集成有原版BBR一键安装、魔改BBR一键安装、锐速（lotServer）一键安装，为四合一版本，四个版本可以切换使用。

适用架构：KVM / Xen，不支持OpenVZ（OVZ）。

适用系统：CentOS 7、Debian 8、Debian 9、Ubuntu 16.04、Ubuntu 18.04。