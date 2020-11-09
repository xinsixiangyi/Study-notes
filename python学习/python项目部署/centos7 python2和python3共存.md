## **centos7 python2和python3共存**



### **一、解决Python2 pip问题**

　　在centos7中安装好操作系统，自带的是Python2的版本，但是并没有pip的方法，我们需要自行安装 报名为python-pip

```ssh
 #默认python2的版本
 [root@operation ~]# python
 Python 2.7.5 (default, Aug 4 2017, 00:39:18)[GCC 4.8.5 20150623 (Red Hat 4.8.5-16)] on linux2Type "help", "copyright", "credits" or "license" for more information.>>> 
 # 安装Python2的pip
 [root@operation ~]# yum install epel-release -y
 [root@operation ~]# yum -y install python-pip 
 # 安装完成后不是最新的pip版本要进行升级
 [root@operation ~]# pip install --upgrade pip 
 # 测试
 [root@operation ~]# pip -V(大写V)pip 18.1 from /usr/lib/python2.7/site-packages/pip (python 2.7)
 # 现在可以使用pip进行对Python2 进行安装Python包了
 # 第一种方法：
 [root@operation ~]# pip install 包名 
 # 第二种方法：
 [root@operation ~]# python -m pip install pymongo (安装Python2的包) 
 # 若是安装的Python3
 [root@operation ~]# python3 -m pip install pymongo (安装Python3的包)

```

### **二、安装Python3**

#### 安装依赖关系

```ssh
[root@operation ~]# yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make 
注：不能忽略相关包，我之前就没有安装readline-devel导致执行python模式无法使用键盘的上下左右键；
```



#### 下载源码包

```SSH
[root@operation ~]# wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tar.xz 
注：如果没有wget命令可以使用 yum -y install wget 安装注：我这里安装的是3.6.6的Python版本 如果想要安装其他的版本可以直接修改版本号
```





#### 解压、编译、安装

```SSH
# 解压
[root@operation ~]# tar -xvJf Python-3.6.6.tar.xz 
# 编译
[root@operation ~]# cd Python-3.6.6
[root@operation Python-3.6.6]# ./configure prefix=/usr/local/python3 
# 安装
[root@operation Python-3.6.6]
# make && make install 
注：没有报错及安装成功，如果报错可以看看是不是一些依赖包没有安装 自行解决不了可以留言评论或者直接联系我
```



　##如果报错zipimport.ZipImportError: can't decompress data; zlib not available make: *** [install] 错误 1

（1）-zipimport.ZipImportError: can't decompress data; zlib not available

一般位于编译安装时出错

缺少zlib依赖包

解决方法：

yum install zlib* -y

重新安装：

make install

#### 设置软连接

```ssh
# 安装完成还是不可以直接在终端输入python3 进入编译器的，我们需要设置软链接[root@operation Python-3.6.6]# ln -s /usr/local/python3/bin/python3 /usr/bin/python3 
# 这样直接执行Python3 就可以进入Python3版本的解释器了
[root@operation Python-3.6.6]# python3
Python 3.6.6 (default, Oct 12 2018, 12:02:11)[GCC 4.8.5 20150623 (Red Hat 4.8.5-28)] on linuxType "help", "copyright", "credits" or "license" for more information.
>>>
```

#### 配置Python3的pip

```ssh
# 设置完python执行后 python3的pip还是不能的用的，也是需要设置的软链接才可以的，在python3的解压目录下是有pip3的命令的
[root@operation Python-3.6.6]# cd /usr/local/python3/bin/
[root@operation bin]#ll pip*
-rwxr-xr-x 1 root root 232 10月 12 12:08 pip
-rwxr-xr-x 1 root root 232 10月 12 12:08 pip3
-rwxr-xr-x 1 root root 232 10月 12 12:08 pip3.6
# 我们需要做个软链接即可
[root@operation bin]# ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3 
# 安装完成后不是最新的pip3版本要进行升级
[root@operation ~]# pip3 install --upgrade pip
```

<span style='color:red;background:yellow;font-size:字体大小;font-family:微软雅黑;'>pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.</span> 

pip的版本和SSL的认证支持有问题，大概的问题是 要想用pip先要支持ssl，但是支持ssl要先更新pip，所以是个矛盾问题， 一键解决方案：更新pip

命令行输入

curl https://bootstrap.pypa.io/get-pip.py | python

但是，注意如果你用的是python3，需要将python写成python3 （本人踩了一小时坑，才总结出来的。。。）

curl https://bootstrap.pypa.io/get-pip.py | python3

**问题修复**

*yum install openssl-devel zilb-devel python3-devel*

```ssh
# 测试
[root@operation bin]# pip3 -V
pip 18.1 from /usr/local/python3/lib/python3.6/site-packages/pip (python 3.6) 
# 使用
[root@operation bin]# pip3 install 包名 
或者
[root@operation bin]# python3 -m pip install 包名
```

　　

### **三、安装TAB补全的解释器(ipython)**

####  安装(我这里安装双版本的)

```ssh
# 安装Python2的ipython
# 第一种方法
[root@operation ~]# pip install ipython
# 第二种方法
[root@operation ~]# python -m pip install ipython
# 安装Python3的ipython
# 第一种方法
[root@operation ~]# pip3 install ipython
# 第二种方法
[root@operation ~]# python3 -m pip install ipython 
注：安装无报错安装成功
```

####  双版本设置软链接

```
# 因为是安装了Python的双版本而且安装的包名都叫 ipython 所有我们执行ipython的时候使用的是安装的python2的版本，我们要使用双版本就要使用软链接
 
[root@operation ~]# ipython
Python 2.7.5 (default, Aug  4 2017, 00:39:18)
Type "copyright", "credits" or "license" for more information.
 
IPython 5.8.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.
 
In [1]:
 
 
 
# 设置Python3的ipython 使用软链接
[root@operation ~]# ln -s /usr/local/python3/bin/ipython /usr/bin/ipython3
```

#### 测试

```
# Python2的ipython
[root@operation ~]# ipython
Python 2.7.5 (default, Aug  4 2017, 00:39:18)
Type "copyright", "credits" or "license" for more information.
 
IPython 5.8.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.
 
In [1]:
 
 
# Python3的ipython
[root@operation ~]# ipython3
Python 3.6.6 (default, Oct 12 2018, 12:02:11)
Type 'copyright', 'credits' or 'license' for more information
IPython 7.0.1 -- An enhanced Interactive Python. Type '?' for help.
 
In [1]: 
```

![img](D:\work\notbook\xinsixiangyi7@163.com\bc352c2f94894602811faede9f1fd61b\49-788521031.png)