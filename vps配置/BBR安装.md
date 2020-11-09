## BBR安装

本文介绍如何在[CentOS](https://www.linuxidc.com/topicnews.aspx?tid=14) 7上部署Google BBR的过程步骤，希望对大家有所参考。【注：文章当时使用的内核版本是4.9.0 而目前是4.15.6（4.15版本高于4.9）】

### **步骤 1: 使用 Elrepo RPM 存储库升级内核**

为了使用 BBR, 您需要将 CentOS 7 机器的内核升级到4.9.0以上。 您可以使用 Elrepo RPM 存储库轻松地完成该操作。

在升级之前, 您可以查看当前内核:

uname -r

此命令应输出字符串, 类似于:

3.10.0-514.2. 2. El7 x86_64

如您所见, 当前内核是3.10.0。

安装 ELRepo repo:

sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

sudo rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm

使用 ELRepo repo安装4.9.0 内核:【当前安装的是4.15.6】

sudo yum --enablerepo=elrepo-kernel install kernel-ml -y

确认结果:

rpm -qa | grep kernel

如果安装成功, 您应该在输出列表中看到kernel-ml-4.9.0-1.el7.elrepo.x86_64

kernel-ml-4.9.0-1.el7.elrepo.x86_64

kernel-3.10.0-514.el7.x86_64

kernel-tools-libs-3.10.0-514.2.2.el7.x86_64

kernel-tools-3.10.0-514.2.2.el7.x86_64

kernel-3.10.0-514.2.2.el7.x86_64

【没有看到没有关系】

现在, 您需要通过设置默认的 GRUB2 启动项来启用4.9.0 内核。

显示 Grub2 菜单中的所有条目:

sudo egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'

结果应类似于:

CentOS Linux 7 Rescue a0cbf86a6ef1416a8812657bb4f2b860 (4.9.0-1.el7.elrepo.x86_64)

CentOS Linux (4.9.0-1.el7.elrepo.x86_64) 7 (Core)

CentOS Linux (3.10.0-514.2.2.el7.x86_64) 7 (Core)

CentOS Linux (3.10.0-514.el7.x86_64) 7 (Core)

CentOS Linux (0-rescue-bf94f46c6bd04792a6a42c91bae645f7) 7 (Core)

由于行计数从0开始, 并且4.9.0 内核项位于第二行上, 因此将默认启动项设置为 1:

sudo grub2-set-default 1

　　注：我得到的结果与示例不同，如下

　　CentOS Linux (4.15.6-1.el7.elrepo.x86_64) 7 (Core)

　　CentOS Linux 7 Rescue 186e68c8657e4bfc8df5044d08b50231 (3.10.0-693.17.1.el7.x86_64)

　　CentOS Linux (3.10.0-693.17.1.el7.x86_64) 7 (Core)

　　CentOS Linux (3.10.0-693.11.6.el7.x86_64) 7 (Core)

　　CentOS Linux (3.10.0-693.el7.x86_64) 7 (Core)

　　CentOS Linux (0-rescue-c73a5ccf3b8145c3a675b64c4c3ab1d4) 7 (Core)

　　此时的CentOS Linux (4.15.6-1.el7.elrepo.x86_64) 7 (Core)位于第一条 所以我输入的应该是

sudo grub2-set-default 0

重新启动系统:

sudo shutdown -r now

当服务器重新联机时, 重新登录并重新运行 uname 命令以确认您使用的是正确的内核:

uname -r

您应该看到如下结果:

4.9.0-1.el7.elrepo.x86_64

（现在应该是）

4.15.6-1.el7.elrepo.x86_64

### **步骤 2: 启用 BBR**

为了启用 BBR 算法, 您需要修改 Sysctl 配置, 如下所示:

echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf

echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf

sudo sysctl -p

现在, 您可以使用以下命令来确认启用了 BBR:

sudo sysctl net.ipv4.tcp_available_congestion_control

输出应类似于:

net.ipv4.tcp_available_congestion_control = bbr cubic reno

下一步, 验证:

sudo sysctl -n net.ipv4.tcp_congestion_control

输出应为:

bbr

最后, 检查内核模块是否已加载:

lsmod | grep bbr

输出将类似于:

tcp_bbr 16384 0

### **步骤 3 (可选): 测试网络性能增强**

为了测试 BBR 的网络性能增强, 您可以在 Web 服务器目录中创建一个文件以供下载, 然后, 从台式计算机上的 Web 浏览器测试下载速度。

```shell
sudo yum install httpd -y

sudo systemctl start httpd.service

sudo firewall-cmd --zone=public --permanent --add-service=http

sudo firewall-cmd --reload

cd /var/www/html

sudo dd if=/dev/zero of=500mb.zip bs=1024k count=500
```

最后, 从桌面计算机上的 Web 浏览器访问 URL http://[your-server-IP]/500mb.zip , 然后评估执行下载速度。

OK。 谢谢你的阅读。