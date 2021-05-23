# python2.7安装MySQLdb模块（以及安装出现的问题和解决方式）

![img](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

[奈斯菟咪踢呦](https://blog.csdn.net/qq_34288630) 2018-05-10 14:30:18 ![img](https://csdnimg.cn/release/blogv2/dist/pc/img/articleReadEyes.png) 3321 ![img](https://csdnimg.cn/release/blogv2/dist/pc/img/tobarCollect.png) 收藏

分类专栏： [Python](https://blog.csdn.net/qq_34288630/category_7613902.html)

版权

## 1、安装MySQLdb

我的Python的版本为2.7，需要额外安装的库有两个，一个是Beautiful Soup，（解析网页）一个是MySQLdb（连接mysql）

进入https://download.csdn.net/download/qq_34288630/10407440
下载
MySQLdb

下载号双击exe：
![这里写图片描述](https://img-blog.csdn.net/2018051014251586?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjg4NjMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

此时遇到问题
安装MySQL-python-1.2.4b4.win32-py2.7.exe的时候，出现python version 2.7 required,which was not found in the registry，翻译过来就是：不能再注册表中识别出来python2.7，解决方案如下：

新建register.py，用IDLE工具打开，复制以下代码进去，并且运行 ，如果显示 — Python 2.7 is now registered! 表示配置成功，如下图：
代码：

```
#
# script to register Python 2.0 or later for use with win32all
# and other extensions that require Python registry settings
#
# written by Joakim Loew for Secret Labs AB / PythonWare
#
# source:
# http://www.pythonware.com/products/works/articles/regpy20.htm
#
# modified by Valentine Gogichashvili as described in http://www.mail-archive.com/distutils-sig@python.org/msg10512.html

import sys

from _winreg import *

# tweak as necessary
version = sys.version[:3]
installpath = sys.prefix

regpath = "SOFTWARE\\Python\\Pythoncore\\%s\\" % (version)
installkey = "InstallPath"
pythonkey = "PythonPath"
pythonpath = "%s;%s\\Lib\\;%s\\DLLs\\" % (
    installpath, installpath, installpath
)

def RegisterPy():
    try:
        reg = OpenKey(HKEY_CURRENT_USER, regpath)
    except EnvironmentError as e:
        try:
            reg = CreateKey(HKEY_CURRENT_USER, regpath)
            SetValue(reg, installkey, REG_SZ, installpath)
            SetValue(reg, pythonkey, REG_SZ, pythonpath)
            CloseKey(reg)
        except:
            print "*** Unable to register!"
            return
        print "--- Python", version, "is now registered!"
        return
    if (QueryValue(reg, installkey) == installpath and
        QueryValue(reg, pythonkey) == pythonpath):
        CloseKey(reg)
        print "=== Python", version, "is already registered!"
        return
    CloseKey(reg)
    print "*** Unable to register!"
    print "*** You probably have another Python installation!"

if __name__ == "__main__":
    RegisterPy()123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051
```

![这里写图片描述](https://img-blog.csdn.net/2018051014265485?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjg4NjMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后重新安装：
![这里写图片描述](https://img-blog.csdn.net/20180510142843790?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjg4NjMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

就能识别出python2.7了。点击下一步安装成功就可以用了

验证是否按安装成功：
打开python.exe
输入命令：

> > > import MySQLdb
> > > 如果不报错则安装成功
> > > ![这里写图片描述](https://img-blog.csdn.net/20180511104731711?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0Mjg4NjMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)