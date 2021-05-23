## 一、安装Apache2.4服务

###  1、下载apache24安装包

将Apache24文件夹移入C:\Program Files (x86)\Apache Software Foundation中

  ![image-20210428150812265](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210428150812265.png)

### 2、修改配置文件

修改配置文件Apache2.4\conf\httpd.conf

![image-20210428150822671](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210428150822671.png)



Listen 80(修改监听端口)

ServerName localhost:80（就是Apache 配置时要绑定的域名，如果没有域名，你可以填写localhost或者127.0.0.1 ，然后通过你 VirtualHost 的IP地址访问。）

C:\Program Files (x86)\Apache Software Foundation\Apache2.4\bin加入到环境变量。

 

需要安装服务，打开cmd，httpd -k install -n apache2.4（管理员权限运行）

 

如未添加环境变量，则：

打开cmd，在C:\Program Files(x86)\Apache Software Foundation\Apache2.2\bin下运行命令如下：

Httpd.exe -k install -n apache2.4

 

安装成功之后再启动Apache

命令行操作的方式如下：

Httpd.exe  -k start 启动服务(添加环境变量可将.exe去掉)

Httpd.exe  -k restart 重启服务

Httpd.exe  -k stop  关闭服务

更多操作，可以自己查询。

 

启动服务之后进行测试，输入127.0.0.1回车提示如下内容，说明安装成功。

 

 

 

### 3、发布Django网站

进入到 C:\Python27\Lib\site-packages\pywin32_system32下，把pythoncom27.dll pythoncomloader27.dll pywintypes27.dll三个库文件拷贝到C:\Python27目录下。

 

 

把mod_wsgi.so拷贝到C:\Program Files(x86)\Apache Software Foundation\Apache2.2\modules目录下。

 

配置httpd.conf文件

进入到C:\Program Files(x86)\Apache Software Foundation\Apache2.2\conf目录下，用编辑器打开httpd.conf文件，在文件的最后一行加入下面代码：

 

```powershell
WSGIApplicationGroup %{GLOBAL}
#添加mod_wsgi.so 模块
LoadModule wsgi_module modules/mod_wsgi.so
#指定myweb项目的wsgi.py配置文件路径
WSGIScriptAlias / C:/Users/Administrator/Desktop/PROJECT/code/sansuo/web/wsgi.py

#指定项目路径
WSGIPythonPath C:/Users/Administrator/Desktop/PROJECT/code/sansuo

#指定python路径
WSGIPythonHome C:/Python27

<Directory C:/Users/Administrator/Desktop/PROJECT/code/sansuo>
<Files wsgi.py>
    Require all granted
</Files>
</Directory>


Alias /static C:/Users/Administrator/Desktop/PROJECT/code/sansuo/static
<Directory C:/Users/Administrator/Desktop/PROJECT/code/sansuo/static>   
    AllowOverride None  
    Options None  
    Require all granted  
</Directory> 
```

 

 

路径根据自己的实际情况更改。

 

安装上有什么问题到C:\Program Files(x86)\Apache Software Foundation\Apache2.4\logs下的error.log查看错误原因。

 

在用Apache调试的时候的注意事项：

修改完代码以后，一定要重启Apache服务器。

### 4、常见问题：

##### 1. LoadModule wsgi_module modules/mod_wsgi.so报错

mod_wsgi.so版本需和python和apache的版本匹配

##### 2.启动apache服务报缺少缺少vcruntime140.dll

下载VC2015运行库。注意下载位数，要与自己apache版本的位数相同，

64位可直接运行文件夹中

 

##### 3.服务启动，但是无法访问到django项目

打开setting.py将debug设置true错误信息会显示到apache的error.log文件中,否则再项目中的documents/log/mysite.log文件中查看错误

## 二、apache安装https服务

 

### 1、修改conf/httpd.conf

 取消ssl及proxy相关注释

```powershell
#LoadModule ssl_module modules/mod_ssl.so （去掉前面的#号）

#Include conf/extra/httpd-ssl.conf （去掉前面的#号）

#LoadModule proxy_module modules/mod_proxy.so （去掉前面的#号）

#LoadModule proxy_http_module modules/mod_proxy_http.so （去掉前面的#号）

#LoadModule socache_shmcb_module modules/mod_socache_shmcb.so s（去掉前面的#号）
```

### 2、生成证书

cmd进入命令行，进入apache安装目录的bin目录。

#### 2.1 设置OPENSSL_CONFIG配置

  执行命令：set OPENSSL_CONF=..\conf\openssl.cnf

![image-20210428163842008](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210428163842008.png)

#### 2.2 生成服务端的key文件

  执行命令：openssl genrsa -out server.key 1024

![image-20210428163900416](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210428163900416.png)

完成后，会在bin目录下生成server.key文件

#### 2.3 生成签署申请

  执行命令：openssl req -new -out server.csr -key server.key

![image-20210428164245045](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210428164245045.png)

  完成后，会在bin目录下生成server.csr文件，其中 Common Name <eg,YOUR name>[] 需要与配置文件中的ServerName一致，否则apache启动时将会报错。

#### 2.4 生成CA的key文件

  执行命令：openssl genrsa -out ca.key 1024

![image-20210428164205969](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210428164205969.png)

完成后，会在目录bin下生成ca.key文件

#### 2.5 生成CA自签署证书

执行命令：openssl req -new -x509 -days 365 -key ca.key -out ca.crt

![image-20210428164639223](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210428164639223.png)

完成后，会在目录bin下生成ca.crt文件，此处填写的信息与2.3步中类似。

#### 2.6手动创建相关目录：

  在bin下新建demoCA文件夹

  bin/demoCA

  在demoCA下新建index.txt

  bin/demoCA/index.txt

  在demoCA下新建serial.txt，其内容为01,重命名删除.txt后缀

  bin/demoCA/serial

  在demoCA下新建newcert文件夹

  bin/demoCA/newcerts

  完成后，执行2.7的命令，会在bin目录下生成server.crt文件。demoCA目录的最终结构如下：

![https://img-blog.csdn.net/20180110142921477?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGRjcWZ5bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast](file:///C:/Users/罗永强/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

#### 2.7 生成CA的服务器签署证书

执行命令：openssl ca -in server.csr -out server.crt -cert ca.crt -keyfile ca.key

![image-20210428164859926](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210428164859926.png)



 此处如果没有创建好相关目录，将会报如下错误：

![img](https://img-blog.csdn.net/20180110142903358?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGRjcWZ5bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 3、修改conf/extra/httpd-ssl.conf文件

#### 3.1 修改https端口号

  根据需要修改httpd-ssl.conf的默认端口号"443"，这里将所有的443修改为"6443"，同时修改ServerName。

  具体如下：

  Listen 6443

  <VirtualHost _default_:6443>

  ServerName localhost

  提示：此处如果保持https默认的443端口，则访问的时候，无需再加端口号。

 

#### 3.2 修改相关证书路径

  在apache安装目录的conf目录下，新建一个key目录，名称随意，然后将bin目录中的相关证书复制到key目录中。key目录最终的文件结构如下：

![https://img-blog.csdn.net/20180110143005893?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGRjcWZ5bA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast](file:///C:/Users/罗永强/AppData/Local/Temp/msohtmlclip1/01/clip_image002.jpg)

  接着修改conf/extra/httpd-ssl.conf文件中的如下内容：

  SSLCertificateFile "xxx/conf/key/server.crt"

  SSLCertificateKeyFile "xxx/conf/key/server.key"

  SSLCACertificateFile "xxx/conf/key/ca.crt"

  **#SSLVerifyClient require （去掉前面的#号，进行客户端验证时需要）网上的部署方案有此项，但实际测试加此项会访问不了，所以忽略此项，或许需要在进行调研。）**

  \#SSLVerifyDepth 1 （去掉前面的#号，把10改为1，进行客户端验证时需要)

SSLSessionCache     "dbm:C:/Program Files (x86)/Apache Software Foundation/Apache2.2/logs/ssl_scache"将此注释放开**（待定）**

\#SSLSessionCache    "shmcb:C:/Program Files (x86)/Apache Software Foundation/Apache2.2/logs/ssl_scache(512000)"将词句屏蔽**（待定）**

 