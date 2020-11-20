## **用Tkinter写GUI**

### **1、组件**

本软件中用到了大概5个组件：

#### 1.1、Button按钮

语法如下：创建了名为Play播放的按钮，绑定了命令sound_play。

```python
b1 = ttk.Button(xin, text = 'Play\n播放',  width = 10, command = sound_play)
b1.grid(row=3,column=4, padx=10, pady=10)
```

#### 1.2、Label标签

语法如下：创建了名为Model的标签。

```python
mm1 = ttk.Label(xin, text = 'Model')
mm1.grid(row=0,column=1)
```

#### 1.3、Combobox下拉列表

```python
number = tk.StringVar()
LessonChosen = ttk.Combobox(xin, width=12, textvariable=number, state='readonly')
LessonChosen['values'] = lesson_name     # 设置下拉列表的值
LessonChosen.grid(column=2, row=1)      # 设置其在界面中出现的位置  column代表列   row 代表行
LessonChosen.current(lesson_name.index(str(WW[0])))    # 设置下拉列表默认显示的值，0为 LessonChosen['values'] 的下标值
```

#### 1.4、LabelFrame

```python
labelframe1 = ttk.LabelFrame(xin, text='単語    （第    课，第    个单词）', height = 80, width=300) # 信息区
labelframe1.grid(row=3,column=1, columnspan = 3 , padx=10, pady=10) 
labelframe1.propagate(0) # 使组件大小不变，此时width才起作用
ttk.Label(labelframe1, text = "  ",font = ('Mincho, 20')).pack(fill="both", expand="yes")
```

#### 1.5、Radiobutton单选按钮

```python
#单选按钮radiobutton
v=tk.IntVar()  
v.set(1)
#f1.bind('<KeyPress-1>',word_know)
f1 = tk.Radiobutton(xin,text = 'Know', font =24, variable=v,value=1, command = word_know)  
f1.grid(row=6,column=2)
f1.configure(state='disabled')
#f2.bind('<KeyPress-2>',word_unknow)
f2 = tk.Radiobutton(xin,text = 'Unknow',font =24,variable=v,value=2, command = word_unknow)  
f2.grid(row=6,column=3)
f2.configure(state='disabled')
```

下面贴一段与『Play播放』按钮绑定的sound_play函数的命令：

```python
#Play按钮
def sound_play():
    import winsound
    import time
    import os
    global xin
    global word_list
    if back_canshu == 0:
        if len(word_list_now)==0:
            showinfo(title = 'Message', message = '恭喜，本次单词已经背完！')
            if BSM == 0:
                word_list1 = [R[1:3]]
                for i in range(len(word_list)):
                    if len(word_list[i])>=6:
                        if word_list[i][5]!='':
                            word_list1.append(word_list[i])
                del word_list
                word_list =word_list1
                #保存file.txt
                import os
                os.remove(r'.\file.txt')
                #写出单词表信息
                write_file_word(word_list,r'.\file.txt')
        else:
            if len(word_list_now)>=1:
                lesson = int(word_list_now[0][0])
                mn = int(word_list_now[0][1])
                if os.path.exists(r".\audio\L"+str(lesson)+"\L"+str(lesson)+"_"+str(mn)+".wav")==False:
                    winsound.PlaySound(r".\audio\Windows_Background.wav", winsound.SND_FILENAME|winsound.SND_ASYNC)
                    #第三行：labelframe单词
                    labelframe1 = ttk.LabelFrame(xin, text='単語    （第 '+str(lesson)+' 课，第 '+str(mn)+' 个单词）', height = 80, width=300) # 信息区
                    labelframe1.grid(row=3,column=1, columnspan = 3 , padx=10, pady=10) 
                    labelframe1.propagate(0) # 使组件大小不变，此时width才起作用
                    ttk.Label(labelframe1, text = word_list_now[0][2], font = ('Mincho, 20')).pack(fill="both", expand="yes")
                else:
                    winsound.PlaySound(r".\audio\L"+str(lesson)+"\L"+str(lesson)+"_"+str(mn)+".wav", winsound.SND_FILENAME|winsound.SND_ASYNC)
    else:
        lesson = int(word_back[-back_canshu][0])
        mn = int(word_back[-back_canshu][1])
        if os.path.exists(r".\audio\L"+str(lesson)+"\L"+str(lesson)+"_"+str(mn)+".wav")==False:
            winsound.PlaySound(r".\audio\Windows_Background.wav", winsound.SND_FILENAME|winsound.SND_ASYNC)
        else:
            winsound.PlaySound(r".\audio\L"+str(lesson)+"\L"+str(lesson)+"_"+str(mn)+".wav", winsound.SND_FILENAME|winsound.SND_ASYNC)
```

### 2、python2和python3的区别

#### 2.1、导入

python2导入

```python
from Tkinter import *
```

python3导入

```python
from tkinter import *
```

### **3、软件封装打包**

thon Tkinter打包封装的方法有：PyInstaller, py2exe, wxPython等方法。答主只是用了PyInstaller来打包，感觉特别好用，对其它方法不做评论。

网上有很多打包方法：[Python | 用Pyinstaller打包发布exe应用](https://link.zhihu.com/?target=http%3A//jingyan.baidu.com/article/a378c960b47034b3282830bb.html)，请自行参考！

要说的是，打包过程中可以添加如下指令，自行尝试打包后的差异：

-w指令，在指令内加入-w命令可以屏蔽发布的exe应用带命令行调试窗口；

-F指令，使用-F指令可以把应用打包成一个独立的exe文件，否则是一个带各种dll和依赖文件的文件夹；

-i指令，可以自定义图标。

**简单写一下用pyinstaller打包软件的过程：**

#### 3.1、pyinstaller安装

```shell
pip install pyinstaller
```

在python包网站下载.whl文件安装

https://www.lfd.uci.edu/~gohlke/pythonlibs/#wordcloud

pip install xxxxx.whl

#### 3.2、pyinstaller使用

①准备好打包文件：

![img](https://pic4.zhimg.com/50/v2-4674887cb14cc6ccf166539f256525cf_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-4674887cb14cc6ccf166539f256525cf_1440w.jpg?source=1940ef5c)

aiueo.py为代码，audio文件夹、tango文件夹和file.txt为程序关联调用的文件，a.ico为软件图标。

②在安装的Python文件夹下的Scripts目录下（包含pyinstaller.exe），shift+右键>>在此处打开命令窗口：

![img](https://pic2.zhimg.com/50/v2-7899acef826f5cc4b16d0f56ca4f86a8_hd.jpg?source=1940ef5c)![img](https://pic2.zhimg.com/80/v2-7899acef826f5cc4b16d0f56ca4f86a8_1440w.jpg?source=1940ef5c)

③输入：pyinstaller.exe -i C:\Users\Naples\aiueo\a.ico -w C:\Users\Naples\aiueo\aiueo.py 回车。

-i C:\Users\Naples\aiueo\a.ico  为图标所在的目录；

-w C:\Users\Naples\aiueo\aiueo.py 为程序所在目录。

![img](https://pic4.zhimg.com/50/v2-1b7bb14294b7b783210c936b358bb85e_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-1b7bb14294b7b783210c936b358bb85e_1440w.jpg?source=1940ef5c)

④在C:\Python34\Scripts目录下就会出现这三个文件：

![img](https://pic4.zhimg.com/50/v2-1d7b505feeb2ac836bded396b594da38_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-1d7b505feeb2ac836bded396b594da38_1440w.jpg?source=1940ef5c)

可以将第一、三个文件删除，只留下dist文件夹。将和程序关联的audio文件夹、tango文件夹和file.txt三个文件夹拷贝到dist/aiueo/目录下。双击aiueo.exe，程序就可以使用了。

可以发送桌面快捷方式，也可以将软件打包拷贝到相同平台下的电脑上使用。

![img](https://pic4.zhimg.com/50/v2-5a18905abd27a6a2ed636ba2e686dc26_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-5a18905abd27a6a2ed636ba2e686dc26_1440w.jpg?source=1940ef5c)

### **4、学习代码**

#### 4.1、字符串转MD5

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from tkinter import *
import hashlib
import time
# from .netscan.test00 import formatForm,LoadingBar
LOG_LINE_NUM = 0

class MY_GUI():
    def __init__(self,init_window_name):
        self.init_window_name = init_window_name


    #设置窗口
    def set_init_window(self):
        self.init_window_name.title("文本处理工具_v1.2")           #窗口名
        #self.init_window_name.geometry('320x160+10+10')                         #290 160为窗口大小，+10 +10 定义窗口弹出时的默认展示位置
        self.init_window_name.geometry('1068x681+10+10')
        #self.init_window_name["bg"] = "pink"                                    #窗口背景色，其他背景色见：blog.csdn.net/chl0000/article/details/7657887
        #self.init_window_name.attributes("-alpha",0.9)                          #虚化，值越小虚化程度越高
        #标签
        self.init_data_label = Label(self.init_window_name, text="待处理数据")
        self.init_data_label.grid(row=0, column=0)
        self.result_data_label = Label(self.init_window_name, text="输出结果")
        self.result_data_label.grid(row=0, column=12)
        self.log_label = Label(self.init_window_name, text="日志")
        self.log_label.grid(row=12, column=0)
        #文本框
        self.init_data_Text = Text(self.init_window_name, width=67, height=35)  #原始数据录入框
        self.init_data_Text.grid(row=1, column=0, rowspan=10, columnspan=10)
        self.result_data_Text = Text(self.init_window_name, width=70, height=49)  #处理结果展示
        self.result_data_Text.grid(row=1, column=12, rowspan=15, columnspan=10)
        self.log_data_Text = Text(self.init_window_name, width=66, height=9)  # 日志框
        self.log_data_Text.grid(row=13, column=0, columnspan=10)
        #按钮
        self.str_trans_to_md5_button = Button(self.init_window_name, text="字符串转MD5", bg="lightblue", width=10,command=self.str_trans_to_md5)  # 调用内部方法  加()为直接调用
        self.str_trans_to_md5_button.grid(row=1, column=11)


    #功能函数
    def str_trans_to_md5(self):
        src = self.init_data_Text.get(1.0,END).strip().replace("\n","").encode()
        #print("src =",src)
        if src:
            try:
                myMd5 = hashlib.md5()
                myMd5.update(src)
                myMd5_Digest = myMd5.hexdigest()
                #print(myMd5_Digest)
                #输出到界面
                self.result_data_Text.delete(1.0,END)
                self.result_data_Text.insert(1.0,myMd5_Digest)
                self.write_log_to_Text("INFO:str_trans_to_md5 success")
            except:
                self.result_data_Text.delete(1.0,END)
                self.result_data_Text.insert(1.0,"字符串转MD5失败")
        else:
            self.write_log_to_Text("ERROR:str_trans_to_md5 failed")


    #获取当前时间
    def get_current_time(self):
        current_time = time.strftime('%Y-%m-%d %H:%M:%S',time.localtime(time.time()))
        return current_time


    #日志动态打印
    def write_log_to_Text(self,logmsg):
        global LOG_LINE_NUM
        current_time = self.get_current_time()
        logmsg_in = str(current_time) +" " + str(logmsg) + "\n"      #换行
        if LOG_LINE_NUM <= 7:
            self.log_data_Text.insert(END, logmsg_in)
            LOG_LINE_NUM = LOG_LINE_NUM + 1
        else:
            self.log_data_Text.delete(1.0,2.0)
            self.log_data_Text.insert(END, logmsg_in)


def gui_start():
    init_window = Tk()              #实例化出一个父窗口
    ZMJ_PORTAL = MY_GUI(init_window)
    # 设置根窗口默认属性
    ZMJ_PORTAL.set_init_window()
    # formatForm(ZMJ_PORTAL, 400, 300)
    # loading = LoadingBar()
    # # 展示滚动条,指定速度
    # loading.show(speed=5)
    init_window.mainloop()          #父窗口进入事件循环，可以理解为保持窗口运行，否则界面不展示


gui_start()
```

#### 4.2、翻译

```python
from tkinter import *
import requests
import json

#根据用户输入翻译
def translation():
    entry1.delete(0,END)
    text = entry.get()
    # text = input("请输入要翻译的内容...")
    url = "http://fanyi.sogou.com/reventondc/translate"
    data = {'from': 'auto', 'to': 'en', 'client': 'pc', 'fr': 'browser_pc', 'text': text, 'useDetect': 'on',
            'useDetectResult': 'on', 'needQc': '1', 'uuid': '2d9c20da-29b2-4d2e-ab24-f0f431d38a33', 'oxford': 'on',
            'isReturnSugg': 'off'}
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/67.0.3396.99 Safari/537.36',
        'Referer': 'http://fanyi.sogou.com/'}
    response = requests.post(url, data=data, headers=headers)
    html_str = response.content.decode()
    dict_ret = json.loads(html_str)
    result = dict_ret['translate']['dit']
    res.set(result)


#创建窗口
root = Tk()

#窗口标题
root.title("中英互译")

#窗口大小、小写x
root.geometry('390x100+500+300')

#标签控件
lable = Label(root,text = "请输入要翻译的内容：",font = ("微软雅黑"),fg = "red")
lable.grid()
lablel1 = Label(root,text = "翻译后的内容为：",font = ("微软雅黑"),fg = "green")
lablel1.grid()

res = StringVar()

#输入控件
entry = Entry(root,font = ("微软雅黑",14))
entry.grid(row = 0,column = 1)
#翻译之后的结果
entry1 = Entry(root,font = ("微软雅黑",14),textvariable = res)
entry1.grid(row = 1,column = 1)

#按钮控件   sticky对齐方式  N S E W - 上下左右
button = Button(root,text = "翻译",font = ("微软雅黑",13),command = translation)
button.grid(row = 2,column = 0,sticky = W)
button1 = Button(root,text = "退出",font = ("微软雅黑",13),command = root.quit)
button1.grid(row = 2,column = 1,sticky = E)

#消息循环、显示窗口
root.mainloop()
```

#### 4.3、进度条

```python
#_*_coding:utf-8_*_
'''
===================================
	进度条.py
	======================
	@descript:

	@copyright:Topsec
	@author:xfjing
	@date:2020/12/7   12:28
===========================================
'''

import tkinter as tk
import time

# 创建主窗口
window = tk.Tk()
window.title('进度条')
window.geometry('630x150')

# 设置下载进度条
tk.Label(window, text='下载进度:', ).place(x=50, y=60)
canvas = tk.Canvas(window, width=465, height=22, bg="white")
canvas.place(x=110, y=60)


# 显示下载进度
def progress():
	# 填充进度条
	fill_line = canvas.create_rectangle(1.5, 1.5, 0, 23, width=0, fill="green")
	x = 500  # 未知变量，可更改
	n = 465 / x  # 465是矩形填充满的次数
	for i in range(x):
		n = n + 465 / x
		canvas.coords(fill_line, (0, 0, n, 60))
		window.update()
		time.sleep(0.02)  # 控制进度条流动的速度

	# 清空进度条
	fill_line = canvas.create_rectangle(1.5, 1.5, 0, 23, width=0, fill="white")
	x = 500  # 未知变量，可更改
	n = 465 / x  # 465是矩形填充满的次数

	for t in range(x):
		n = n + 465 / x
		# 以矩形的长度作为变量值更新
		canvas.coords(fill_line, (0, 0, n, 60))
		window.update()
		time.sleep(0)  # 时间为0，即飞速清空进度条


btn_download = tk.Button(window, text='启动进度条', command=progress)
btn_download.place(x=400, y=105)

window.mainloop()
if __name__ == '__main__':
	pass
```



#### 4.4、网络扫描

```python
#_*_coding:utf-8_*_
'''
===================================
	scan.py
	======================
	@descript:

	@copyright:Topsec
	@author:xfjing
	@date:2020/12/8   12:00
===========================================
'''
# coding:utf-8
from Tkinter import *
import tkinter.messagebox
from ScrolledText import ScrolledText  # 文本滚动条
import threading
import time,socket,datetime
from PIL import ImageTk, Image
try:
	import configparser
except:
	import ConfigParser as configparser

import requests, time, json
import urllib3

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)


class Scanning():
	def __init__(self, goal_ip, tool_ip):
		self.__goal_ip = goal_ip
		self.__tool_ip = tool_ip
		pass

	# 创建任务
	def creaat_task(self):
		text.insert(END, '扫描中，请稍后。。。\n')
		tool_ip = self.__tool_ip
		ip_list = self.__goal_ip
		goal_ip_list = ''
		if len(ip_list) == 1:
			goal_ip_list = ip_list[0]
			pass
		elif len(ip_list) > 1:
			for p in ip_list:
				if goal_ip_list:
					goal_ip_list = goal_ip_list + "," + p
				else:
					goal_ip_list = p
		name = int(time.time())
		# 漏洞分类列表
		vuln_list = ['browser_vuln', 'email_vuln', 'ftp_vuln', 'web_vuln', 'dns_vuln', 'tcp/ip_vuln', 'rpc_vuln',
					 'smtp_vuln', 'weak_password_vuln', 'windows_vuln', 'nfs_vuln', 'router_switch_vuln',
					 'ftp_share_vuln']
		# 漏洞分类关键词
		vuln_key = [['浏览器', 'Browser', 'Firefox', 'internet'], ['邮件', 'email'], ['FTP'], ['Web服务', 'Apache', 'PHP'],
					['DNS'], ['TCP', 'IP'], ['RPC'], ['SMTP'], ['弱口令'],
					['Windows', 'ICMP', '操作系统'], ['NFS'], ['路由器', 'router', '交换机', 'switch'], ['文件共享', 'share']]
		data_list = []

		# 使用用户名：superman 密码：talent登录，获取登录用户id
		get_url = 'https://{0}/home/Api/Register/restLogin/?name=superman&password=talent'.format(tool_ip)
		# 设置请求头
		head = {'Connection': 'close'}
		# 发送请求，设置verify为False,关闭ssl验证
		login_resuit = requests.get(get_url, timeout=30, headers=head, verify=False)
		# 获取登陆用户唯一标识
		print(login_resuit.json())
		authid = login_resuit.json()['data']['authid']

		# 创建任务，以时间为名字
		creat_url = 'https://{3}/home/Api/TaskManage/addVulTask/?userMark={0}&scan_name={1}&target_hosts={2}'.format(
			authid, name, goal_ip_list, tool_ip)
		head = {'Connection': 'close'}
		creat_result = requests.get(creat_url, timeout=30, headers=head, verify=False)
		print creat_result.json()
		task_uuid = creat_result.json()['data']['uuid']
		# 查询当前任务状态
		try:
			while True:
				statu_find_url = 'https://{2}/home/Api/TaskManage/showTaskStatus/?userMark={0}&uuid={1}'.format(
					authid, task_uuid, tool_ip)
				statu_result = requests.get(statu_find_url, timeout=30, headers=head, verify=False)
				statu_index = statu_result.json()['rows']['status']
				if statu_index == '4':
					break
					pass
				else:
					# print("==")
					text.insert(END, '扫描中，请稍后。。。\n')
					time.sleep(5)
				pass
			ip_url = 'https://{3}/home/Api/VulTaskResult/showHost/?userMark={0}&uuid={1}'.format(authid, task_uuid,
																								 goal_ip_list, tool_ip)
			ip_request = requests.get(ip_url, timeout=300, headers=head, verify=False)
			task_ip = ip_request.json()['rows']
			# print(task_ip)
			if task_ip:
				list_ip = [i['ip_address'] for i in task_ip]

				for goal_ip in list_ip:
					# print(goal_ip)
					data_dict = {
						"ip": goal_ip,
						"tcp_port": [],
						"udp_port": [],
						"port_service_map": {},
						"os": "",
						"application_flag_map": {},
						"browser_vuln": [],
						"email_vuln": [],
						"ftp_vuln": [],
						"web_vuln": [],
						"dns_vuln": [],
						"tcp/ip_vuln": [],
						"rpc_vuln": [],
						"smtp_vuln": [],
						"weak_password_vuln": [],
						"windows_vuln": [],
						"trojan": [],
						"nfs_vuln": [],
						"router_switch_vuln": [],
						"ftp_share_vuln": [],
					}

					# 获取当前扫描IP下的所有漏洞_
					vuln_url = 'https://{3}/home/Api/VulTaskResult/showHostNvts/?userMark={0}&uuid={1}&ip_address={2}'.format(
						authid, task_uuid, goal_ip, tool_ip)
					vuln_request = requests.get(vuln_url, timeout=300, headers=head, verify=False)
					# c=json.dumps(vuln_request.json()).decode("unicode-escape")
					# a=r.json()['rows']['hosts'][0]['os']
					b = vuln_request.json()['rows']
					# print(b)
					for f in range(len(b)):
						d = json.dumps(b[f]).decode("unicode-escape")
						try:
							vuln_name = json.loads(d, strict=False)['nvt_name'].encode('utf-8')
						except:
							continue
							pass
						# print(vuln_name)
						if '木马' in vuln_name:
							if '漏洞' in vuln_name:
								pass
							elif '端口' in vuln_name:
								pass
							else:
								data_dict['trojan'].append(vuln_name)
							pass
						for i in range(len(vuln_key)):
							for a in range(len(vuln_key[i])):
								if vuln_key[i][a] in vuln_name:
									# if 'CVE' in vuln_name:
									# 	data_dict[vuln_list[i]].append('CVE' + vuln_name.split('CVE')[-1].split(')')[0])
									if json.loads(d, strict=False)['cve'].encode('utf-8'):
										data_dict[vuln_list[i]].append(
											json.loads(d, strict=False)['cve'].encode('utf-8'))

					port_url = 'https://{3}/home/Api/VulTaskResult/showHostInfo/?userMark={0}&uuid={1}&ip_address={2}'.format(
						authid, task_uuid, goal_ip, tool_ip)
					port_request = requests.get(port_url, timeout=300, headers=head, verify=False)
					os = port_request.json()['data']['os']

					# application_flag_map ={}
					# application_flag_map.
					application_flag_map = port_request.json()['data']['application_flag_map']
					port_list = port_request.json()['data']['port_list']
					tcp_port = []
					udp_port = []
					service = {}
					for port_index in range(len(port_list)):

						if port_list[port_index]['status'] == 'open':
							service.update({port_list[port_index]['port'] + '/' + port_list[port_index]['protocol']:
												port_list[port_index]['service']})
							if port_list[port_index]['protocol'] == 'tcp':
								tcp_port.append(port_list[port_index]['port'])
							elif port_list[port_index]['protocol'] == 'udp':
								udp_port.append(port_list[port_index]['port'])
					data_dict.update({"os": os})
					data_dict.update({"application_flag_map": application_flag_map})
					data_dict.update({"tcp_port": tcp_port})
					data_dict.update({"udp_port": udp_port})
					data_dict.update({"port_service_map": service})
					data_list.append(data_dict)

			else:
				data_dict = {
					"ip": '',
					"tcp_port": [],
					"udp_port": [],
					"port_service_map": {},
					"os": "",
					"application_flag_map": {},
					"browser_vuln": [],
					"email_vuln": [],
					"ftp_vuln": [],
					"web_vuln": [],
					"dns_vuln": [],
					"tcp/ip_vuln": [],
					"rpc_vuln": [],
					"smtp_vuln": [],
					"weak_password_vuln": [],
					"windows_vuln": [],
					"trojan": [],
					"nfs_vuln": [],
					"router_switch_vuln": [],
					"ftp_share_vuln": [],
				}
				data_list.append(data_dict)
		except Exception as x:

			print(x)
			print('任务异常')
		finally:
			# 无论是否完成任务，将创建好的任务删除
			del_url = 'https://{2}/home/Api/TaskManage/deleteTask/?userMark={0}&uuid={1}'.format(
				authid, task_uuid, tool_ip)
			time.sleep(10)
			del_task = requests.get(del_url, timeout=30, headers=head, verify=False)
			if del_task.json()['result'] == True:
				print ('已删除任务')
			else:
				print ('删除任务失败')
				pass
		return data_list

# date=[{
#"ip":"192.168.1.3",
#"tcp_port":["20","22","23","443"],
#"udp_port":["53","69","123","524"],
#"port_service_map":{"20/tcp":"ftp","23/tcp":"telnet","53/udp":"dns"},
#"os":"",
#"application_flag_map":{"office":"flag1","visio":"flag2","chrome":"flag1"},
#"browser_vuln":["CVE-2018-9335","CVE-2016-7239","CVE-2016-7196"],
#"email_vuln":[],
#"ftp_vuln":["CVE-2019-7230","CVE-2019-9600"],
#"web_vuln":["CVE-2019-2889","CVE-2019-2891"],
#"dns_vuln":["CVE-2019-16057","CVE-2018-20994"],
#"tcp/ip_vuln":["CVE-2014-7880","CVE-2014-4076"],
#"rpc_vuln":["CVE-2019-16935","CVE-2018-10927"],
#"smtp_vuln":["CVE-2019-11458","CVE-2018-6789"],
#"weak_password_vuln":["CVE-2010-4733","CVE-2009-2317"],
#"windows_vuln":["CVE-2019-1361","CVE-2019-1060"],
#"trojan":["Ghost","Win32.Hack.Huigezi"],
#"nfs_vuln":["CVE-2019-11277","CVE-2018-15797"],
#"router_switch_vuln":["CVE-2019-3930","CVE-2019-5285"],
#"ftp_share_vuln":["CVE-2006-1161","CVE-2005-3641"],
#"scan_packet":"34"
# }]
list=["ip", "tcp_port","udp_port", "port_service_map","os","application_flag_map","browser_vuln","email_vuln","ftp_vuln","web_vuln","dns_vuln", "tcp/ip_vuln","rpc_vuln","smtp_vuln","weak_password_vuln", "windows_vuln","trojan", "nfs_vuln","router_switch_vuln", "ftp_share_vuln"]
show_dict={"space_dict":
			{ "ip":"未扫描到本机IP", "tcp_port": "未扫描到开放的tcp端口","udp_port":"未扫描到开放的udp端口", "port_service_map":"无端口映射关系","os":"未检测到系统信息", "application_flag_map":"无相关应用旗标","browser_vuln":"无相关漏洞","email_vuln":"无相关漏洞","ftp_vuln":"无相关漏洞","web_vuln":"无相关漏洞","dns_vuln":"无相关漏洞", "tcp/ip_vuln":"无相关漏洞","rpc_vuln": "无相关漏洞","smtp_vuln": "无相关漏洞","weak_password_vuln":"无相关漏洞","windows_vuln":"无相关漏洞","trojan":"无相关漏洞","nfs_vuln":"无相关漏洞","router_switch_vuln":"无相关漏洞","ftp_share_vuln":"无相关漏洞"},
		"key_dict":
			{ "ip":"ip地址",
		"tcp_port": "tcp端口",
		"udp_port":"udp端口",
		"port_service_map":"端口映射",
		"os":"系统信息",
		"application_flag_map":"应用旗标",
		"browser_vuln":"browser漏洞",
		"email_vuln":"email漏洞",
		"ftp_vuln":"ftp漏洞",
		"web_vuln":"web漏洞",
		"dns_vuln":"dns漏洞",
		"tcp/ip_vuln":"tcp/ip漏洞",
		"rpc_vuln": "rpc漏洞",
		"smtp_vuln": "smtp漏洞",
		"weak_password_vuln":"弱口令漏洞",
		"windows_vuln":"windows漏洞",
		"trojan":"木马",
		"nfs_vuln":"nfs漏洞",
		"router_switch_vuln":"路由转换器漏洞",
		"ftp_share_vuln":"文件共享漏洞"}
		}
def nativeIp():
	#获取计算机名称
	hostname=socket.gethostname()
	#获取本机IP
	ip=socket.gethostbyname(hostname)

	return ip

def receive(ip,tools_ip):
	start_time = int(time.time())
	# 此处调用方法 输入为靶机ip地址 输出为客户需求中的data部分数据
	# 定义接收数据列表
	scanning = Scanning(goal_ip=ip, tool_ip=tools_ip)
	dataValue = scanning.creaat_task()
	date = dataValue
	# 记录运行结束时间
	end_time = int(time.time())
	#计算时间差
	t1 = time.localtime(start_time)
	t2 = time.localtime(end_time)
	t1 = time.strftime("%Y-%m-%d %H:%M:%S", t1)
	t2 = time.strftime("%Y-%m-%d %H:%M:%S", t2)
	time1 = datetime.datetime.strptime(t1, "%Y-%m-%d %H:%M:%S")
	time2 = datetime.datetime.strptime(t2, "%Y-%m-%d %H:%M:%S")
	return date,(time2 - time1).seconds
	pass
def count():
	ip = nativeIp()
	cf = configparser.ConfigParser()
	cf.read("./config.ini")
	tool_ip = cf.get("network_scan", "tool_ip")
	# print tool_ip,ip
	# try:
	date,sum_time=receive(ip=[ip],tools_ip=tool_ip)
	# cnt=0
	# while True:
	# 	date, sum_time = receive(ip=ip, tools_ip=tool_ip)
	# 	text.insert(END, '扫描中，请稍后。。。\n')
	# 	if date:
	# 		break
	text.delete('1.0', END)
	time.sleep(1)
	# text.insert(END, '扫描结果。。。')
	# text.tag_config(1.0,justify=CENTER)
	text.insert(INSERT, '扫描结果\n')
	text.tag_add("tag1", "1.0",)
	text.tag_config("tag1",justify=CENTER)
	# a = data["data"][0]
	# cnt = 0
	# for x in a.keys():
	# 	tree.insert("", cnt, text="line{0}".format(cnt), values=(x, a.get(x),))
	# 	cnt += 1
	# cnt = 0
	for x in list:
		if date[0][x]:
			text.insert(END, "{0}:  {1}\n".format(show_dict['key_dict'][x], date[0][x]))
		else:
			text.insert(END, "{0}:  {1}\n".format(show_dict['key_dict'][x],show_dict['space_dict'][x]))
	text.insert(END, '总耗时{0}：\n'.format(sum_time))
	# except Exception as x:
	# 	print(x)
	# 	text.delete('1.0', END)
	# 	time.sleep(1)
	# 	text.insert(INSERT, '扫描失败，请关闭软件重新扫描\n')
	# 	text.tag_add("tag1", "1.0", )
	# 	text.tag_config("tag1", justify=CENTER)
	# 	pass


def fun():
	th = threading.Thread(target=count)
	th.setDaemon(True)  # 守护线程
	th.start()
	var.set('TOPSEC')
	button.config(state=DISABLED)



root = Tk()
root.title('网络扫描工具')  # 窗口标题
root.geometry('+200+5')  # 窗口呈现位置
image2 = Image.open(r'logo.png')
background_image = ImageTk.PhotoImage(image2)
textlabel = Label(root, image=background_image)
textlabel.grid()
text = ScrolledText(root, font=('微软雅黑', 10), fg='blue')
text.grid()
button = Button(root, text='开始扫描本机', font=('微软雅黑', 10), command=fun)
button.grid()
var = StringVar()  # 设置变量
label = Label(root, font=('微软雅黑', 10), fg='red', textvariable=var)
label.grid()
var.set('点击上方开始扫描')
root.mainloop()
```

