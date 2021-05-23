## python线程池

### **1.threading模块**

threading模块是众多多线程管理模块的其一，它能确保重要的子线程退出后进程才退出。

multiprocess模块的完全模仿了threading模块的接口，二者在使用层面，有很大的相似性，因而不再详细介绍

------

### **2.创建线程的两种方式**

方式一：

```
from threading import Thread
import time
# 进程等待所有线程结束后才会结束

def func():
    print('线程 start')
    time.sleep(2)
    print('线程 end')

if __name__ == '__main__':

    t = Thread(target=func)
    t.start() # 告诉操作系统开一个线程
    print('主')
```

方式二：

```
from threading import Thread
import time
class Myt(Thread):
    def run(self):
        print('子线程 start')
        time.sleep(2)
        print('子线程 end')

t = Myt()
t.start()
print('主线程'
```

------

### **3.子进程和子线程pid的比较**

```
from threading import Thread
from multiprocessing import Process
import os

def func():
    print('hello',os.getpid())

if __name__ == '__main__':
    # 在主进程开启多个线程，每个线程都跟主进程pid一样
    t1 = Thread(target=func)
    t2 = Thread(target=func)
    t1.start()
    t2.start()
    print('主线程/主进程pid:',os.getpid())

    # 开个多个子进程，每个进程都有不同的pid：
    p1 = Process(target=func)
    p2 = Process(target=func)
    p1.start()
    p2.start()
    print('主线程/主进程pid:', os.getpid())
```

------

### **4.子线程内存数据共享问题**

```
from threading import Thread
from multiprocessing import Process

def func():
    global n
    n = 0

if __name__ == '__main__':
    # 子进程：
    n = 100
    p = Process(target=func)
    p.start()
    p.join()
    print('主',n)  # 毫无疑问子进程p已经将自己的n改成了全局的n,改成了0，但改的仅仅是它自己的，查看父进程的n仍然为100

    # 子线程：
    n = 1
    t=Thread(target=func)
    t.start()
    t.join()
    print('主',n) # 查看结果为,因为同一进程内的线程之间共享进程内的数据
```

------

### **5.线程的join方法**

```
from threading import Thread
import time


def func(name,n):
    print(f'{name} start')
    time.sleep(n)
    print(f'{name} end')


t1 =Thread(target=func,args=('线程1',1))
t2 =Thread(target=func,args=('线程2',2))
t3 =Thread(target=func,args=('线程3',3 ))
start = time.time()
t1.start()
t2.start()
t3.start()

t1.join()   # 等待子线程运行结束
t2.join()
t3.join()
end = time.time() #
print(end-start)   # 3.0060362
```

------

### **6.Thread类的其他方法**

- `isAlive()`：返回线程是否活动的。
- `getName()`：返回线程名。
- `setName()`：设置线程名。

threading模块提供的一些方法：

- `threading.currentThread()`：返回当前的线程变量。
- `threading.enumerate()`：返回一个包含正在运行的线程的list。正在运行指线程启动后、结束前，不包括启动前和终止后的线程。
- `threading.activeCount()`：返回正在运行的线程数量，与len(threading.enumerate()) 有相同的结果。

```
from threading import Thread,currentThread,enumerate,activeCount
import time

def task():
    print('子线程 start')
    time.sleep(2)
    print('子线程 end')
    print(enumerate())
    # print(currentThread(),'子线程')
if __name__ == '__main__':
   t1 = Thread(target=task)
   t2 = Thread(target=task)
   t1.start()
   t2.start()



   # print(t1.is_alive()) # True
   # print(t1.getName()) # Thread-1
   # print(t2.getName()) # Thread-2
   # t1.setName('班长')
   # print(t1.getName())
   print(currentThread().name)
   # print(enumerate()) # [<_MainThread(MainThread, started 1856)>, <Thread(Thread-1, started 6948)>, <Thread(Thread-2, started 3128)>]
   print(activeCount()) # 3
   # print(len(enumerate())) # 3
```

------

### **7.多线程实现socket**

server服务端

```
import socket
from threading import Thread

s=socket.socket()
s.bind(('127.0.0.1',8080))
s.listen(5)

def run(conn):
    while True:
        try:
            data = conn.recv(1024)
            print(data)
            conn.send(data.upper())
        except Exception:
            break

if __name__ == '__main__':
    while True:
        print('等待客户端连接')
        conn,addr = s.accept()
        print(f'客服端{addr}连接成功')
        t = Thread(target=run,args=(conn,))
        t.start()
```

client客户端

```
import socket
s = socket.socket()
s.connect(('127.0.0.1',8080))
while True:
    msg = input('>>>:').strip()
    if not msg:
        continue
    s.send(msg.encode('utf-8'))
    data = s.recv(1024)
    print(data.decode('utf-8'))
```