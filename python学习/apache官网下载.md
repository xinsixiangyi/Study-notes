如何从Apache官网下载windows版apache服

[分步阅读](http://jingyan.baidu.com/album/29697b912f6539ab20de3cf8.html)

由于个人有强迫倾向，下载软件都喜欢从官网下载，摸索了好久终于摸清楚怎么从Apache官网下载windows安装版的Apache服务器了，现在分享给大家。

![img](D:\work\notbook\xinsixiangyi7@163.com\672b5a5167bb49d2886c03fddca36c27\f82065fb7219.png)

工具/原料

- apache

方法/步骤

1. 1

进入apache服务器官网http://httpd.apache.org/，这里我们以下载稳定版的

httpd 2.2.29为例，点击download。

![img](D:\work\notbook\xinsixiangyi7@163.com\e9fe33639d254c52aab376deefa8550a\83aee9d76d19.png)

1. 2

**由于官方网页改版，以前的方式可能五法进行下载，为了不浪费大家的时间，特此修正2017-09-22**

第一步点击左边download链接

![img](D:\work\notbook\xinsixiangyi7@163.com\a6217d0de2ce4ccbb64c8112091d32ae\935652bb741e.png)

1. 3

点击链接 a number of third party vendors

后面的步骤就和之前的一致了

![img](D:\work\notbook\xinsixiangyi7@163.com\2b3353be5f6b4a33a002875473528c51\056105a36e1e.png)

1. 4

The Apache HTTP Server Project itself does not provide binary releases of software, only source code. Individual committers may provide binary packages as a convenience, but it is not a release deliverable.

If you cannot compile the Apache HTTP Server yourself, you can obtain a binary package from numerous binary distributions available on the Internet.

Popular options for deploying Apache httpd, and, optionally, PHP and MySQL, on Microsoft Windows, include：

ApacheHaus

Apache Lounge

BitNami WAMP Stack

WampServer

XAMPP

大致意思是说apache本身不提供已编译的安装包，只提供源码，如果你自己无法编译，可以选择下面这些官方推荐的第三方提供编译的网站。

其中后两个是有名的wamp以及xampp集成环境，如果只想下载apache可以选择前三个网站，这里我们第一个ApacheHaus为例。

![img](D:\work\notbook\xinsixiangyi7@163.com\f7aa91f94a93410aac46f97ee6fd6658\00fc76f79719.png)

1. 5

打开ApacheHaus之后你会发现这个网站上有各种windows版本，可以尽情选择你要下载的版本。

![img](D:\work\notbook\xinsixiangyi7@163.com\6aa4ead03e634dba889961882e80f62a\cd8921c58f19.png)

1. 6

点击红框中的图标即可开始下载，x86是32位的，x64是64位的，根据自己的操作系统选择下载

![img](D:\work\notbook\xinsixiangyi7@163.com\8bb9751826c14ac2b536573400bcc12a\450789018919.png)

1. 7

解压后是一个压缩包，把他移动到你想放置的地方。

![img](D:\work\notbook\xinsixiangyi7@163.com\ba3314aa842c49a7ba774da8fd579a84\d5413b8c8419.png)

![img](D:\work\notbook\xinsixiangyi7@163.com\fb3db94352a24fbbb88111e613fa506b\94fc518c8019.png)

1. 8

命令行下进入到apache下面的bin目录，输入

httpd -k install

把apache安装成windows后台服务。

![img](D:\work\notbook\xinsixiangyi7@163.com\a61a7a05053a408680a800b1c6572f4b\8a775cddfc19.png)

1. 9

利用ApacheMonitor来启动你的apache

The Apache Monitor is a desktop tray application that allows you to monitor the existence of a running Apache service and easily start, stop and restart Apache. To use it just double click on the ApacheMonitor.exe in the \Apache24\bin folder. If you want it to start automatically for you when you log into the computer, just drag a copy into the Startup folder in Window's Start Menu.

Apache监控器是一个允许你用来监控正在运行的Apache服务的软件，并且让你启动、停止和重启Apache变得更容易。只需要双击\Apache24\bin目录下的

ApacheMonitor.exe就可以运行该程序，如果你想开机自动启动该软件，把该软件拉倒开始菜单里的Startup folder即可（windows10的Startup folder使用方法请自行百度）

![img](D:\work\notbook\xinsixiangyi7@163.com\efa740a82a624ec5ac9b66a593c2f4e4\e7ef2906f919.png)

1. 10

更多操作请参考附带的说明文档readme_first.html。

![img](D:\work\notbook\xinsixiangyi7@163.com\7d3aeb6267d14ed8b040e1c81453a604\b6f39087f219.png)

1. 11

**针对大家遇到的一些问题做下简单总结**

1、由于apache默认是监听80端口，如果你的电脑iis是启动状态，并且也使用了80端口，apache将无法正常启动，需要先停止iis，另外迅雷也可能会使用80端口，所以也要关闭迅雷。查看80端口是否被占用，命令行下输入：

netstat -aon|findstr "80"

如果看到如图的结果，说明80端口已被使用，需要先关闭相关软件，或者修改apache默认的监听端口

打开apache目录下的conf/httpd.conf  搜索  "Listen 80"

修改为Listen 8088

保存之后再重新启动apache

![img](D:\work\notbook\xinsixiangyi7@163.com\82e8cca9192843b7a4cf1b5dbf0ff31b\a010bd33c219.png)

![img](D:\work\notbook\xinsixiangyi7@163.com\342f8ceefece43a3b41686396dbf4ef8\8689a0463b1e.png)

1. 12

2、httpd -k install 输入该命令后查看服务是否安装成功

开始-->运行-->services.msc-->确定

打开后如果在服务列表能够看到apache字样，说明，服务安装成功，可以直接点击左边的启动按钮来启动服务

![img](D:\work\notbook\xinsixiangyi7@163.com\8cbb839470a64a00b9ded6a7509869ac\3c4132ba321e.png)

![img](D:\work\notbook\xinsixiangyi7@163.com\fb216359ac094459a1ccde7236015808\3bef344f1e1e.png)

1. 13

3、服务无法正常安装，首先确定软件32位和64位是否和自己的系统匹配

其次，看电脑有没有安装软件相应的运行包，比如你下载的是VC9版本，那么你需要先安装Microsoft Visual C++ 2008 Redistributable ，同理VC11版本你需要安装Microsoft Visual C++ 2012 Update 4 Redistributable Package (X86 & x64)

在ApacheHaus网站的最底部提供了相应的Visual Studio Redistributable Packages下载链接

![img](D:\work\notbook\xinsixiangyi7@163.com\1587c6051911448a92863157008e035e\dfb2dd19171e.png)

END

注意事项

- 如果下载方法过时请留言通知我，以便我及时更新，最后更新于2017-09-22

经验内容仅供参考，如果您需解决具体问题(尤其法律、医学等领域)，建议您详细咨询相关领域专业人士。

举报作者声明：本篇经验系本人依照真实经历原创，未经许可，谢绝转载。