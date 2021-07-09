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
$ git remote - v
origin  git@github.com:username /Animations .git (fetch)
origin  git@github.com:username /Animations .git (push)
```

五、从远程获取最新版本到本地

使用如下命令可以在本地新建一个temp分支，并将远程origin仓库的master分支代码下载到本地temp分支

```
$ git fetch origin master:temp
remote: Counting objects: 18, done .
remote: Compressing objects: 100% (6 /6 ), done .
remote: Total 11 (delta 3), reused 0 (delta 0)
Unpacking objects: 100% (11 /11 ), done .
From github.com:username /Animations
  * [new branch]      master     -> temp
    c07bdc7..40f902d  master     -> origin /master
```

六、用git pull来更新代码的时候，遇到了下面的问题：

```powershell
error: Your local changes to the following files would be overwritten by merge:
    xxx/xxx/xxx.php
Please, commit your changes or stash them before you can merge.
Aborting
```

出现这个问题的原因是其他人修改了xxx.php并提交到版本库中去了，而你本地也修改了xxx.php，
这时候你进行git pull操作就好出现冲突了，解决方法，在上面的提示中也说的很明确了。

1、保留本地的修改 的改法

​	1）直接commit本地的修改 ----也一般不用这种方法

​	2）通过git stash ---- 通常用这种方法

```shell
git stash
git pull
git stash pop
```

通过git stash将工作区恢复到上次提交的内容，同时备份本地所做的修改，之后就可以正常git pull了，git pull完成后，执行git stash pop将之前本地做的修改应用到当前工作区。

git stash: 备份当前的工作区的内容，从最近的一次提交中读取相关内容，让工作区保证和上次提交的内容一致。同时，将当前的工作区内容保存到Git栈中。

git stash pop: 从Git栈中读取最近一次保存的内容，恢复工作区的相关内容。由于可能存在多个Stash的内容，所以用栈来管理，pop会从最近的一个stash中读取内容并恢复。

git stash list: 显示Git栈内的所有备份，可以利用这个列表来决定从那个地方恢复。

git stash clear: 清空Git栈。此时使用gitg等图形化工具会发现，原来stash的哪些节点都消失了。

2、放弃本地修改 的改法 ----这种方法会丢弃本地修改的代码，而且不可找回

``` shell
git reset --hard
git pull
```

