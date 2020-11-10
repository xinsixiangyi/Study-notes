**centos7下syslog（rsyslog）服务器配置**

修改配置文件

```shell
vim /etc/rsyslog.conf
```

rsyslog.conf文件内容

```shell

# Provides UDP syslog reception

#$ModLoad imudp                                                  去掉#

#$UDPServerRun 514                                               去掉#

# Provides TCP syslog reception

#$ModLoad imtcp                                                   去掉#

#$InputTCPServerRun 514                                			  去掉#

#配置接受文件地址及名称

$template IpTemplate,"/var/log/syslog_xf/%FROMHOST-IP%.log"         

*.*  ?IpTemplate

& ~

#排除本地主机IP日志记录，只记录远程主机日志

fromhost-ip, !isequal, "127.0.0.1"

?Remote

& ~
```

\#其他不用改

\#重启syslog服务 

```shell
systemctl restart rsyslog
```

```shell
#查看是否开启监听

netstat -antup | grep 514

sudo netstat -tulpn | grep rsyslog

firewall-cmd --permanent --zone=public --add-port=514/udp

firewall-cmd --permanent --zone=public --add-port=514/tcp

firewall-cmd --reload
```

```shell
#查看所有打开的端口：

 firewall-cmd --zone=public --list-ports

#更新防火墙规则： 

firewall-cmd --reload

#查看防火墙状态

firewall-cmd --state

#停止firewall

systemctl stop firewalld.service

#禁止firewall开机启动

systemctl disable firewalld.service 
```

