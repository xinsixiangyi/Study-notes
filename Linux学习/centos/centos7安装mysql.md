**centos7安装MySQL**

[**centos7 安装 Mysql 5.7.27，详细完整教程**](https://www.cnblogs.com/jinghuyue/p/11565564.html)

\1. 下载 MySQL yum包

```shell
wget http://repo.mysql.com/mysql57-community-release-el7-10.noarch.rpm 
```

2.安装MySQL源

```shell
rpm -Uvh mysql57-community-release-el7-10.noarch.rpm
```

 

3.安装MySQL服务端,需要等待一些时间

```shell
yum install -y mysql-community-server
```

 

4.启动MySQL

```shell
systemctl start mysqld.service
```

 

5.检查是否启动成功

```shell
systemctl status mysqld.service
```

 

6.获取临时密码，MySQL5.7为root用户随机生成了一个密码

grep 'temporary password' /var/log/mysqld.log 

![img](D:\work\notbook\xinsixiangyi7@163.com\0b66188d14a54d40a3d9bb0b384c7b0f\ru5erkjggg==.png)

 

 

7.通过临时密码登录MySQL，进行修改密码操作

mysql -uroot -p

使用临时密码登录后，不能进行其他的操作，否则会报错，这时候我们进行修改密码操作

 ==注：==如果报错

<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: Yes 或者No)</span>

a,停止mysql服务

```
systemctl stop mysqld.service
```

b，修改配置文件无密码登录

```
vi /etc/my.cnf
```

在最尾部加上

```
skip-grant-tables
```

保存

c，启动mysql

```
systemctl start mysqld.service
```

d，登录musql

```
mysql -u root
```

此处注意不要加-p

e,修改密码，mysql5.7用此语法

```mysql
use mysql ;
update mysql.user set authentication_string=password('123456') where user='root';
FLUSH PRIVILEGES;
```

f,回到第二步骤去掉加上的

```
skip-grant-tables
```

8.因为MySQL的密码规则需要很复杂，我们一般自己设置的不会设置成这样，所以我们全局修改一下

```mysql
mysql> set global validate_password_policy=0;

mysql> set global validate_password_length=1;
```

这时候我们就可以自己设置想要的密码了

 

```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'yourpassword';
```

 

9.授权其他机器远程登录

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'yourpassword' WITH GRANT OPTION;  
FLUSH PRIVILEGES;
```

 

10.开启开机自启动

先退出mysql命令行，然后输入以下命令

```shell
systemctl enable mysqld
systemctl daemon-reload
```

 

11.设置MySQL的字符集为UTF-8，令其支持中文

vim /etc/my.cnf

改成如下,然后保存

 

```shell
# For advice on how to change settings please see
# http://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html
#这两行 
[mysql]
default-character-set=utf8
 
[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock

#还有这两行
default-storage-engine=INNODB
character_set_server=utf8
 
symbolic-links=0
 
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
```

12.重启一下MySQL,令配置生效

```shell
service mysqld restart
```

 

13.防火墙开放3306端口

```shell
firewall-cmd --state 

firewall-cmd --zone=public --add-port=3306/tcp --permanent 

firewall-cmd --reload
```

 

14.卸载MySQL仓库

一开始的时候我们安装的yum，每次yum操作都会更新一次，耗费时间，我们把他卸载掉

rpm -qa | grep mysql

![img](D:\work\notbook\xinsixiangyi7@163.com\601771e8ab324754b75894d1e76db203\suvork5cyii=.png)

 

yum -y remove mysql57-community-release-el7-10.noarch

 

15.数据库的操作

 

（1）查看mysql是否启动：service mysqld status

启动mysql：service mysqld start

停止mysql：service mysqld stop

重启mysql：service mysqld restart

（2）查看临时密码：grep password /var/log/mysqld.log