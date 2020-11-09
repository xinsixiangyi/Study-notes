**redis安装**

**Linux 源码安装**

**下载地址：**http://redis.io/download，下载最新稳定版本。

本教程使用的最新文档版本为 2.8.17，下载并安装：

```shell
wget http://download.redis.io/releases/redis-6.0.8.tar.gz 
tar xzf redis-6.0.8.tar.gz # cd redis-6.0.8 # make
```

安装gcc套装

yum install cpp yum install binutils yum install glibc yum install glibc-kernheaders yum install glibc-common yum install glibc-devel yum install gcc yum install make 升级gcc

yum -y install centos-release-scl yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils scl enable devtoolset-9 bash

执行完 **make** 命令后，redis-6.0.8 的 **src** 目录下会出现编译后的 redis 服务程序 redis-server，还有用于测试的客户端程序 redis-cli：

下面启动 redis 服务：

```shell
cd src # ./redis-server
```

注意这种方式启动 redis 使用的是默认配置。也可以通过启动参数告诉 redis 使用指定配置文件使用下面命令启动。

```shell
cd src 
./redis-server ../redis.conf
```

**redis.conf** 是一个默认的配置文件。我们可以根据需要使用自己的配置文件。

由于Redis默认的是本地访问保护，所以我们需要修改以下四点

- 注释bind 127.0.0.1 使redis能够被外部访问
- 修改protected-mode 为no 关闭保护模式
- 修改daemonize 为yes 后台运行进程
- 添加requirepass <pass> 添加redis密码

启动 redis 服务进程后，就可以使用测试客户端程序 redis-cli 和 redis 服务交互了。 比如：

```shell
cd src 
./redis-cli redis> set foo bar OK redis> get foo "bar"
```