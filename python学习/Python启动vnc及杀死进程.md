自定义cmd命令行
自定义发出指令
自定义终止程序
识别端口是否被占用
配置文件管理端口使用情况
需优化获取有用端口慢的问题
一个ip地址只可以指定一个端口，

自动识别目标主机是否占据一个端口
自动检测目标主机占据的端口在本地真实环境是否被使用
创建的服务写入配置文件中
如果目标主机占据一个端口，并且该端口正在使用，直接返回True即可，无需创建服务

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-


"""
自定义cmd命令行
自定义发出指令
自定义终止程序
识别端口是否被占用
配置文件管理端口使用情况
需优化获取有用端口慢的问题
一个ip地址只可以指定一个端口，

自动识别目标主机是否占据一个端口
自动检测目标主机占据的端口在本地真实环境是否被使用
创建的服务写入配置文件中
如果目标主机占据一个端口，并且该端口正在使用，直接返回True即可，无需创建服务





"""
import os
import configparser
import socket
import random
import json
import subprocess
import thread
import time
from func_timeout import func_set_timeout
from web.settings import STATIC_ROOT

import sys
#重新加载sys模块，
reload(sys)
#重新设置字符集（此时不会出现提示，别怀疑自己敲错了）
sys.setdefaultencoding("utf-8")

# cmd = 'ipconfig'
# cmd = 'node C:/Users/Administrator/Desktop/sansuo/static/node_modules/noVNC/websockify-js-master/websockify/websockify.js --web C:/Users/Administrator/Desktop/sansuo/static/node_modules/noVNC 9000 192.168.91.132:5901'
# ps = os.popen(cmd,'r')
# print ps.read()
# time.sleep(10)
# print '关闭服务'
# sys.exit()



class Cigini():

    def __init__(self):
        self.conf = configparser.ConfigParser()
        self.dripath = os.path.dirname(os.path.realpath(__file__))
        self.drinipath = os.path.join(self.dripath,'localport.ini')


    def readiniport(self):
        '''
        获取port，从ini文件中
        :return:port
        '''
        self.conf.read(self.drinipath)
        self.conf.sections()
        self.portlist = self.conf.items('section')[0][1]
        self.portlist = eval(self.portlist)

        while True:
            port = self.portlist[random.randint(0,len(self.portlist))]
            self.port_status = self.checkport(port)
            if self.port_status == False:
                return int(port)

    @func_set_timeout(3)
    def checkport(self,port):
        '''
        验证port是否被占用
        :return: True，False
        '''
        self.local_ip = self.get_host_ip()

        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        try:
            s.connect((self.local_ip,int(port)))
            s.shutdown(2)
            # 利用shutdown()函数使socket双向数据传输变为单向数据传输。shutdown()需要一个单独的参数，
            # 该参数表示了如何关闭socket。具体为：0表示禁止将来读；1表示禁止将来写；2表示禁止将来读和写。
            # print('%d is open' % port)
            return True
        except Exception as e:
            # print('%d is down' % port)
            return False

    @staticmethod
    def get_host_ip():
        """
        查询本机ip地址
        :return:
        """
        try:
            s=socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
            s.connect(('8.8.8.8',80))
            ip=s.getsockname()[0]
        finally:
            s.close()
        return ip


    def readinihost(self):

        """
        read [section1]
        :return:
        """
        self.conf.read(self.drinipath)
        self.hostlist = self.conf.items('section1')[0][1]
        self.hostlist = eval(self.hostlist)

        return self.hostlist

    def writeinihost(self,zjqk):
        """
        write host    ini
        :param host:
        :return:
        """
        self.conf.read(self.drinipath)
        # 以下三个参数必须都为字符串
        self.conf.set('section1','hostserver',json.dumps(zjqk))
        self.conf.write(open(self.drinipath,'r+'))

    @func_set_timeout(3)
    def checkhostport(self,ip,port):
        sk=socket.socket(socket.AF_INET,socket.SOCK_STREAM)
        try:
            sk.connect((ip,int(port)))
            return True
        except Exception:
            return False


# ss = Cigini()
# ss.readinihost()
# ss.writeinihost({'dsa':'dasdadadadas'})


class PoCmd():
    '''
    host:为远程目标主机ip
    port:为远程目标主机vnc服务端口，需自行提供

    '''

    def __init__(self,host,port=5900):
        self.nodepath = r'start /b node {0}\node_modules\noVNC\websockify-js-master\websockify\websockify.js --web {0}\node_modules\noVNC'.format(STATIC_ROOT)
        self.killcmd = 'taskkill /pid '
        self.force = ' /f'
        self.forpid = 'netstat -aon|findstr '
        self.targetaddress = host
        self.port = port
        self.iniserver = Cigini()
        self.localport = self.iniserver.readiniport()
        self.serverhost = self.iniserver.readinihost()

    def __str__(self):

        """
        创建启动命令
            nodepathcmd
        """
        try:
            if not isinstance(self.targetaddress,str):
                self.targetaddress = str(self.targetaddress)
            if not isinstance(self.port,str):
                self.port = str(self.port)
            self.cmd = self.nodepath +' '+str(self.localport) + ' '+ self.targetaddress +':'+self.port
            return  self.cmd
        except:
            return 'Parameter error......'

    def for_in_pid(self,port):

        """
        获取系统进程
            获取当前端口所占用进程
            port：端口号
        """
        address = '0.0.0.0:'+str(port)
        port = '"'+str(port)+'"'
        self.forpid = self.forpid + port
        with os.popen(self.forpid) as res:
            res = res.read().split('\n')
        result = []
        for line in res:
            temp = [i for i in line.split(' ') if i != '']
            if len(temp) > 4:
                result.append({'pid': temp[4], 'address': temp[1], 'state': temp[3]})
        for i in result:
            if i['address'] == address:
                pid = i['pid']
        return pid

    def killprocess(self,port):
        """
        杀死端口所开进程
            port：端口号

        """
        pid = self.for_in_pid(port)
        killpidcmd = self.killcmd + str(pid) + self.force
        code = os.popen(killpidcmd,'r')
        if code.read().decode('gbk') == u'成功: 已终止 PID 为 '+str(pid)+' 的进程。' or code.read().decode('gbk') == '':
            return True
        else:
            return False


    def commond(self):

        """
        在cmd中启动node服务
        

        """
        try:
            self.cmd = self.__str__()
            # print self.cmd
            subprocess.Popen(self.cmd, shell=True)
            self.serverhost[self.targetaddress] = self.localport
            self.iniserver.writeinihost(self.serverhost)
            return True
        except:
            # print 'Service startup failed'
            return False



    def acquiceport(self):

        """
        启动服务，返回result
            会先去ini文件中找port占用情况，如果存在，则去判定系统进程中是否真实存在

        """

        if self.iniserver.checkhostport(self.targetaddress,self.port):
            if self.targetaddress in self.serverhost.keys():
                if self.iniserver.checkport(self.serverhost[self.targetaddress]) == True:
                    #如果服务端口开启，，
                    # 那抹这是第二次访问
                    # 需要重新顶掉上一个服务，重新开启新服务
                    serstatus = self.killprocess(self.serverhost[self.targetaddress])
                    self.localport = self.serverhost[self.targetaddress]
                    sestr = self.commond()
                    # print sestr
                else:
                    self.localport = self.serverhost[self.targetaddress]
                    sestr = self.commond()
            else:
                sestr = self.commond()
            if sestr == True:
                return self.string(sestr,self.localport)
            else:
                return self.string(sestr,'The service failed to start because the server is out of control')
        else:
            return self.string(False,'The remote computer connection failed')


    def string(self,result,resson):
        '''
        :return
        :return: {'result':True,'port':9000}
                {'result':False,'error':'server start faild'}
        '''
        retuenresult = {}
        if result == True:
            retuenresult['result'] = True
            retuenresult['port'] = self.localport
        else:
            retuenresult['result'] = False
            retuenresult['error'] = resson
        return json.dumps(retuenresult)


if __name__ == '__main__':
    ip = '10.7.215.228'
    port = 5900
    ds = PoCmd(ip,port)
    print json.loads(ds.acquiceport())

    # print ds.killprocess(9080)
```

### 配置文件

```ini
[section]
localport = [9000, 9001, 9002, 9003, 9004, 9005, 9006, 9007, 9008, 9009, 9010, 9011, 9012, 9013, 9014, 9015, 9016, 9017, 9018, 9019, 9020, 9021, 9022, 9023, 9024, 9025, 9026, 9027, 9028, 9029, 9030, 9031, 9032, 9033, 9034, 9035, 9036, 9037, 9038, 9039, 9040, 9041, 9042, 9043, 9044, 9045, 9046, 9047, 9048, 9049, 9050, 9051, 9052, 9053, 9054, 9055, 9056, 9057, 9058, 9059, 9060, 9061, 9062, 9063, 9064, 9065, 9066, 9067, 9068, 9069, 9070, 9071, 9072, 9073, 9074, 9075, 9076, 9077, 9078, 9079, 9080, 9081, 9082, 9083, 9084, 9085, 9086, 9087, 9088, 9089, 9090, 9091, 9092, 9093, 9094, 9095, 9096, 9097, 9098, 9099]

[section1]
hostserver = {"10.7.215.228": 9080, "10.63.70.170": 9014}
```

### 测试ip和端口是否开放

```python
# socket try connect

def PortOpen(request,port):
    toolid= request.GET.get('toolid', '')
    sys_load = Kb_tools.objects.using("KnowledgeBase").get(id=int(toolid))
    ip=sys_load.tooladress
    tool_data=json.loads(sys_load.tooldata)
    port=tool_data.get('port')
    #print('\033[1m*Port\033[0m %s:%d' % (ip, port)),
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    try:
        s.connect((ip, port))
        s.shutdown(2)
        #print('\033[1;32m.... is OK.\033[0m')
        return HttpResponse(True)
    except socket.timeout:
        #print('\033[1;33m.... is down or network time out!!!\033[0m')
        return HttpResponse(False)
    except:
        #print('\033[1;31m.... is down!!!\033[0m')
        return HttpResponse(False)
```



