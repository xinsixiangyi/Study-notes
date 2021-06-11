## celery配置

### 1、安装包

```
celery==3.1.23
redies==2.10.6
#版本过高会导致报错
```

### 2、测试

task.py

```python
#_*_coding:utf-8_*_
'''
===================================
	task.py.py
	======================
	@descript: celery执行函数
	@author:xfjing
	@date:2021/6/5   15:30
===========================================
'''
from celery import Celery
import time
celery_app = Celery('task', broker='redis://:xfjing..@39.102.136.57:6379/2')


@celery_app.task
def sendmail(email):
    print('sending mail to %s...' % email)
    time.sleep(20.0)
    print('mail sent.')

```

### 3、运行 Celery 职程（Worker）服务

```powershell
$ celery -A task worker --loglevel=info #须在task.py文件下task即为我们的文件
```

### 4、调用任务

需要调用我们创建的实例任务，可以通过 `delay()` 进行调用。

`delay()` 是 `apply_async()` 的快捷方法，可以更好的控制任务的执行：

start.py

```python
#_*_coding:utf-8_*_
'''
===================================
	start.py
	======================
	@descript: 调用celery函数执行任务
	@author:xfjing
	@date:2021/6/5   16:43
===========================================
'''
from task import sendmail
if __name__ == '__main__':
	k=sendmail.delay('11111test@ptorch.com')
	print(k)
	k2 = sendmail.delay('22222test@ptorch.com')
	print(k2)
	k3 = sendmail.delay('33333test@ptorch.com')
	print(k3)
	k4 = sendmail.delay('44444test@ptorch.com')
	print(k4)
	pass
```

执行start.py文件

控制台可以看到

```powershell
'delivery_info': {'exchange': 'celery', 'routing_key': 'celery', 'priority': 0, 'redelivered': None}, 'hostname': 'celery
@LAPTOP-AB86NPPQ', 'is_eager': False, 'group': None}) kwargs:{})
[2021-06-05 16:53:13,735: WARNING/Worker-1] sending mail to 44444test@ptorch.com...
[2021-06-05 16:53:13,746: DEBUG/MainProcess] Task accepted: task.sendmail[913aa6de-4e1e-4958-abc9-87176a059b7e] pid:13548

[2021-06-05 16:53:33,668: WARNING/Worker-1] mail sent.
[2021-06-05 16:53:33,668: INFO/MainProcess] Task task.sendmail[d67c16cc-b3cb-4717-8b83-32a900fbade8] succeeded in 20.0s:
None
[2021-06-05 16:53:33,707: WARNING/Worker-1] mail sent.
[2021-06-05 16:53:33,707: INFO/MainProcess] Task task.sendmail[7753a2cd-21d1-4065-996f-81085e94dbba] succeeded in 20.0s:
None
[2021-06-05 16:53:33,720: WARNING/Worker-1] mail sent.
[2021-06-05 16:53:33,720: INFO/MainProcess] Task task.sendmail[7716d60f-e203-4508-a921-3a6ddf015909] succeeded in 20.0s:
None
[2021-06-05 16:53:33,736: WARNING/Worker-1] mail sent.
[2021-06-05 16:53:33,736: INFO/MainProcess] Task task.sendmail[913aa6de-4e1e-4958-abc9-87176a059b7e] succeeded in 20.0s:
None


```

### 5、教程

【中文】https://www.celerycn.io/ru-men/celery-chu-ci-shi-yong

