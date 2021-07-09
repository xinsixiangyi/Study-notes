> **NoVnc**

简介:采用novnc实现远程桌面协助,需在远程主机安装vncserver服务.

安装客户端服务访问：

1.  在windows中安装novnc服务，并使用websockify进行代理发送链接 （我使用的是win7，其他版本应差别不大）

> 需先准备node.js安装包：[[https://nodejs.org/zh-cn/download/releases/]{.underline}](https://nodejs.org/zh-cn/download/releases/)
>
> （尽量选择msi进行安装，安装的时候选择npm package manager）因为接下来为了方便安装ws模块，node.js14.0以上版本已经不支持win7，请根据实际情况选择
>
> Novnc安装：[[https://github.com/novnc/noVNC]{.underline}](https://github.com/novnc/noVNC) (直接github中code下载就好)
>
> Websockify.js安装: [[https://github.com/novnc/websockify-js]{.underline}](https://github.com/novnc/websockify-js) (直接github中code下载就好)

二、安装node.js 选择

> ![](media/image1.png){width="2.40625in" height="0.8333333333333334in"}
>
> 完成后，安装ws,optimist,mime-types
>
> npm install ws
>
> npm install optimist
>
> npm install mime-types
>
> 
>
> 安装完成后，在C:\\Users\\Administrator中会出现一个node\_moudles的文件夹，
>
> 如果文件夹中出现以下相关几个文件夹则表示安装成功 （除了noVNC没有是我后面放进去的）
>
> 

然后将下载完成的noVNC也放入node\_moudles文件夹中，将websockify.js放入noVNC文件夹中，

3.  编辑websockify-js-master\\websockify\\websockify.js文件，修改154行的 filename += \'/index.html\';代码为filename += \'/vnc.html\';

然后启动服务

node C:\\Users\\Administrator\\node\_modules\\noVNC\\websockify

-js-master\\websockify\\websockify.js \--web C:\\Users\\Administrator\\node\_modules\\no

VNC 9000 192.168.91.132:5901



然后在浏览器输入[http://本机ip:9000]{.underline}即可出现以下界面，表示成功（执行这一步前需要现在远程主机安装服务）



在远程主机安装服务（windows）

Ultravnc安装：[[https://www.uvnc.com/downloads/ultravnc.html]{.underline}](https://www.uvnc.com/downloads/ultravnc.html)

安装后打开ultravnc-server，右键点击admin properties



修改authentication 下面的密码，点击ok即可

然后关闭防火墙，或者在防火墙中添加例外5900端口 （端口为vnc服务端口）



完成这一步之后，，，，客户端即可访问

在远程主机安装服务（centos6）

先检查是否安装vnc-servers yum --q tigervnc tigervnc-server



表示未安装服务

查看可以安装的vnc服务：yum list tigervnc-server



Yum安装vnc-servers服务yum install tigervnc-server

安装完成后，启动vncserver



设置远程访问密码

完成后，关闭防火墙服务

systemctl stop firewalld.service

systemctl status firewalld.service

或者需要在防火墙开放端口5901

设置完成后也可在客户端访问
