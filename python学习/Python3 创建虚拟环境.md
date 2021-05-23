## Python3 创建虚拟环境

**python -m venv ../virtualenv/myblog-01**

cd ../virtualenv/testscripts 

activate

cd/d D:\Download\pip-20.0.2 

python setyp.py install  

**先在window系统安装python3，因为venv是python3独有的工具，Mac/Linux系统也一样，Mac上自带python2，python3的安装可参考文章：**

### **1. Windows系统下创建虚拟环境**

**选择建立虚拟环境的文件夹，比如桌面，打开windows的dos界面，去到建立虚拟环境的文件夹，cd '文件夹名'，这里是cd desktop**

**两种方法建立虚拟环境文件夹，比如我的虚拟环境文件夹是python_ven_demo：**

#### **1. 在文件夹下直接建立：**

**先新建文件夹python_ven_demo，**

**然后进入该文件夹cd python_ven_demo，**

**然后搭建虚拟环境：python -m venv . （注：venv 之后一个空格加上一点“.”)**

#### **2. 在桌面上直接建立虚拟环境文件夹 及 虚拟环境：**

**直接输入 python -m venv python_ven_demo (虚拟环境文件夹名，直接在桌面建立了文件夹，同时生成了虚拟环境）**

#### **3. 生成的虚拟环境内容：**

#### **4. 激活虚拟环境**

**[注意：只有激活之后，才算进入该虚拟环境，否则安装包时，依然是安装在全局环境之下]**

**激活文件在Script文件夹下，如图所示。激活：activate.bat，退出：deactivate.bat**

**激活方式为：dos 界面进入python_ven_demo/文件夹，然后输入activate.bat (也可直接输入activate）即可激活环境，输入python 就进入python3.6环境了,注，Max/Linux系统可进入python2.7环境，详见后续介绍**

**退出方式：输入deactivate.bat 或 deactivate**

#### **5. 进行包安装：以jieba(分词包)为例**

**首先需要先激活，在激活后，才可以进行安装，否则安装到的是全局环境下**

**激活后，pip3 install jieba**

**下载好后，可在虚拟环境下看到该包安装在了该虚拟环境下，如果删除了该虚拟环境文件夹，则包页一起被删除了，不会对全局有影响**

### **2. Linux/Mac系统下创建虚拟环境**            

**和windos类似，差别不大，所以只简单叙述过程：**

#### **1. 建立虚拟环境方法相同，进入目标文件夹cd desktop**

**使用python -m venv python_ven_demo**

**或新建文件夹python_ven_demo，进入 cd python_ven_demo，然后python -m venv .**

**可以看到，文件构成与windos略有不同**

#### **2. 激活**

**激活方法和windos不同，activate文件在bin文件夹下，并且，激活时不可直接输入activate[这样是无效的]，而是需要使用source activate 命令：**

#### **3. 包安装**

**同windows，激活后，输入python2，可进入python2.7环境，输入python3，可进入python3.6环境**

**pip install 可安装2.7对应的包，pip3可安装3.6对应的包**