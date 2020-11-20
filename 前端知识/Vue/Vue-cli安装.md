# VUE CLI 安装

### 环境准备

#### **1.1. 安装Node.js **

​				**（建议使用LTS版本）**

​	节点版本要求

Vue CLI 4.x需要[Node.js](https://nodejs.org/)版本8.9或更高版本（建议使用v10 +）。您可以使用[n](https://github.com/tj/n)，[nvm](https://github.com/creationix/nvm)或[nvm-windows](https://github.com/coreybutler/nvm-windows)在同一台计算机上管理多个版本的Node 。

##### **nvm安装：**

###### 	centos安装：

​	国内：

```bash
[root@iZ2zegp1t778obqpfi8885Z npm]# wget  https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
--2020-11-19 14:03:50--  https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh
Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.108.133
Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 13527 (13K) [text/plain]
Saving to: ‘install.sh’

100%[==========================================================================================>] 13,527      24.9KB/s   in 0.5s   

2020-11-19 14:03:53 (24.9 KB/s) - ‘install.sh’ saved [13527/13527]
#授权
[root@iZ2zegp1t778obqpfi8885Z npm]# chmod +x install.sh 
[root@iZ2zegp1t778obqpfi8885Z npm]# ./install.sh 
=> Downloading nvm as script to '/root/.nvm'

=> nvm source string already in /root/.bashrc
=> bash_completion source string already in /root/.bashrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
#更新环境变量
[root@iZ2zegp1t778obqpfi8885Z npm]# source /root/.bashrc

```

使用nvm安装nodejs

由于网络问题，请设置国内源
指定 nvm 的镜像需要在环境配置中增加 NVM_NODEJS_ORG_MIRROR
在/root/.bashrc中增加以下内容

```bash
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
```

刷新环境变量

```bash
source /root/.bashrc
```

查看nodejs可用版本

```bash
nvm ls-remote

```

安装 node

```bash
nvm  install v10.23.0    #Vue CLI 4.x 建议使用v10 +
```

npm换淘宝源

```bash
npm config get registry
#https://registry.npmjs.org/
npm config set registry https://registry.npm.taobao.org
```

安装vue-cli

```bash
npm install -g @vue/cli	
```

==注==:若安装报错使用

```bash
npm install -g --unsafe-perm @vue/cli
```

查看vue cli版本

```bash
vue --version
```

国外：

```bash
[root@bambooxf-2 ~]# wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh |bash
=> Downloading nvm as script to '/root/.nvm'

=> Appending nvm source string to /root/.bashrc
=> Appending bash_completion source string to /root/.bashrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
#更新环境变量
[root@bambooxf-2 ~]# source /root/.bashrc
```



###### windows:

地址nvm:https://github.com/coreybutler/nvm-windows/releases

![img](D:\work\notbook\xinsixiangyi7@163.com\9222d81d997047a990f6f0e78a8262d9\clipboard.png)

![img](D:\work\notbook\xinsixiangyi7@163.com\1df6624948b34f478ac4108b8af987b3\clipboard.png)

但是后面再下载npm的时候一直下载不下来，需要使用国内镜像，修改nvm安装目录下的settings.txt文件，加上两行：

node_mirror: https://npm.taobao.org/mirrors/node/

npm_mirror:  https://npm.taobao.org/mirrors/npm/

就可以下载安装成功了。

#### **1.2安装vue-cli**

```bash
npm install -g @vue/cli	
```



```bash
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

查看vue/cli 版本

创建一个项目：

```bash
vue create hello-world
# OR
vue ui
```

