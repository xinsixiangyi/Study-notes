## 安装git

一、查询Git

​		查看是否安装过git

```bash
git --version
```

​		如果未安装，则出现以下结果

![image-20201125164640871](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20201125164640871.png)

二、安装

```bash
yum -y install git
```

三、将已有项目拷贝到本地

```powershell
git clone https://github.com/xinsixiangyi/Study-notes.git "./"
```

四、查看远程分支

使用如下命令可以查看远程仓库（我这里有一个origin仓库）

```
$ git remote -` `v``origin git@github.com:username` `/Animations` `.git (fetch)``origin git@github.com:username` `/Animations` `.git (push)
```

