# S 7 使用NVM管理nodejs

\###1. 安装nvm

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
1
```

会输出如下：

```bash
=> Downloading nvm as script to '/root/.nvm'

=> Appending nvm source string to /root/.bashrc
=> Appending bash_completion source string to /root/.bashrc
=> Close and reopen your terminal to start using nvm or run the following to use it now:

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
123456789
```

提示信息可以看出，设置了环境变量， 需要刷新环境变量

```bash
source /root/.bashrc
1
```

验证环境变量是否生效

```bash
echo $NVM_DIR
1
```

输出了/root/.nvm说明已经OK
验证nvm安装是否成功

```bash
nvm --version
1
```

输出版本号说明nvm安装Ok

\###2. 使用nvm安装nodejs
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

安装nodejs

```bash
nvm install v8.9.4
```

查看已经安装的所有版本

```bash
nvm ls 
1
```

使用某个版本

```bash
nvm use v8.9.4
```

Win下设置镜像地址：修改安装目录下的settings.txt

```json
root: D:\nvm-windows\nvm\nvm
path: D:\nvm-windows\nvm\nodejs
arch: 64 
proxy: none
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```