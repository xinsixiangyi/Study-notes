## centos7下安装jre

### **1 下载jre**

我们都知道运行程序需要jre，而开发需要jdk，但在服务器上我们只需要jre运行程序即可

首先在[官网](https://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html)下载所需jre，下面是oracle官网提供的下载地址

https://www.oracle.com/technetwork/java/javase/downloads/index-jsp-138363.html

![img](D:\work\notbook\xinsixiangyi7@163.com\233ba50efe5f4402bb4f33b6a510de07\108100208716.png)

在这里我们下载的是server jre而不是jre，首先我现在解释一下 jdk、jre以及Server jre分别是什么吧

**1、jdk**

JDK(Java Development Kit)又称J2SDK(Java2 Software Development Kit)，是Java开发工具包，它提供了Java的开发环境(提供了编译器javac等工具，用于将java文件编译为class文件)和运行环境(提供了JVM和Runtime辅助包，用于解析class文件使其得到运行)。如果你下载并安装了JDK，那么你不仅可以开发Java程序，也同时拥有了运 行Java程序的平台。JDK是整个Java的核心，包括一堆Java工具tools.jar和Java标准类库。

**2、jre**

JRE(Java Runtime Enviroment)是Java的运行环境。面向Java程序的使用者，而不是开发者。JRE是运行Java程序所必须环境的集合，包含JVM标准实现及 Java核心类库。它包括Java虚拟机、Java平台核心类和支持文件。它不包含开发工具(编译器、调试器等)。

**3、server jre****

Server JRE是专为服务器端程序量身打造的, 只包含JRE/JDK中最常用的那部分功能.。为了做到简单，Server JRE不使用安装包, 而是一个绿色版的压缩文件。

**JDK8之后的版本不在包括JRE**。

因此我们选择下载server jre

点击server jre进入下载页面

![img](D:\work\notbook\xinsixiangyi7@163.com\c70cfd295379412988a0818924ac1e10\108100958802.png)

![img](D:\work\notbook\xinsixiangyi7@163.com\cfefccddaa8e43b4a895f94116b1145c\108101045863.png)

下载linux版的jre

### **2 把jre上传服务器**

将我们下载的jre文件通过ftp软件传到服务器，如[xftp](https://www.netsarang.com/zh/free-for-home-school/)

为了传输方便我们先上传在解压，传到/home/java目录，java目录是自己建的

![img](D:\work\notbook\xinsixiangyi7@163.com\d5845b69c46c4abbb2f6160a628ddc5b\108102006801.png)

### **3 解压jre文件**

使用命令解压

tar -xzvf server-jre-8u231-linux-x64.tar.gz

- 1

![img](D:\work\notbook\xinsixiangyi7@163.com\201dac9f8e6c4d598444cd42161c2878\110810224063.png)

解压成功之后可以看到

![img](D:\work\notbook\xinsixiangyi7@163.com\a215e898ac3542188511a1b255db669d\108102344119.png)

这样截解压成功了

### **4 jre目录以及子目录授予root权限**

![img](D:\work\notbook\xinsixiangyi7@163.com\c7b4b2b79dca42cd9ef43efc2e3ff148\108102740652.png)

可以看到jdk这个目录并没有像下面的文件一样有所属用户和所属组

给目录和目录下的子目录授予root权限

chown root:root -R /home/java/jdk1.8.0_231/

- 1

![img](D:\work\notbook\xinsixiangyi7@163.com\ae98dd632e154014880065e43feb8b79\108103553169.png)

### **5 配置环境变量**

编辑环境变量配置文件

```shell
vi /etc/profile 
```

在文件末尾加入下面代码，保存并退出 :wq

```shell
export JAVA_HOME=/home/java

export JRE_HOME=/home/java/jdk1.8.0_231

export CLASSPATH=$JRE_HOME/lib/rt.jar:$JRE_HOME/lib/ext

export PATH=$PATH:$JRE_HOME/bin

```

使环境变量即时生效

source /etc/profile

### **6 测试**

java -version

![img](D:\work\notbook\xinsixiangyi7@163.com\debb23df23f548e0973819f0150ffe78\108104008414.png)

或者java和javac也可以