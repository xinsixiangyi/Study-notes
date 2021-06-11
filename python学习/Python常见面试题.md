# Python常见面试题

## **基础部分**：

### **1、闭包及装饰器作用【实际开发使用】？**

闭包：就是在一个外函数中定义了一个内函数，内函数里运用了外函数的临时变量，并且 外函数的返回值是内函数的引用。

装饰器(本质就是闭包)：主要作用为已经存在的对象添加额外的功能，例如日志记录、数据校验等。

```python
import time
def show_time(f):
    def inner():
        start=time.time()
        f()
        end=time.time()
        print('spend time:%s'%(end-start))
    return inner
@show_time #此处的@show_time相当于function1=show_time(function1)
def function1():
    print('excute function<<')
    time.sleep(2)
function1()
print('---------------------')
@show_time
def function2():
    print('excute function2<<')
    time.sleep(2)
function2()
    #function1=show_time(function1) 相当于返回inner

def function3():
    print('excute function3<<')
    time.sleep(3)
fun=show_time(function3)
fun()
```

### **2、深拷贝及浅拷贝？**

深拷贝使用deepcopy()函数完成（deepcopy的本质是递归 copy）

浅拷贝有三种形式:

切片操作:b = a[:]或者b = [x for x in a];

工厂函数：b = list(a);

copy函数：b = copy.copy(a)

浅拷贝和深拷贝的区别是：浅拷贝只是将原对象在内存中**引用地址**拷贝

而深拷贝是将这个对象的所有内容拷贝过来了，包括值与内存地址

### **3、python2与python3的区别，举5例**

2: 字符串以 8-bit 字符串存储 3改为: 字符串以 16-bit Unicode 字符串存储

2: print是一个类 3改为: print是一个函数

2: xrange() 生成**迭代对象**（range的升级版）3改为：统一使用range() 效率提升了

2: 默认编码是ASCII 3改为: 默认编码是UTF-8

2: long类型 3改为: 去除long

还有一些模块名字的改变例如queue、urllib等

### **4、is和==区别？**

is：比较两边的内存地址是否相等

==：比较两边的数值是否相等

### **5、正则：写手机尾号除了4和7的正则表达式**

```python
re.match("1\d{9}[0-3,5-6,8-9]",str)
```

### **6、有没有接触过多线程？怎么将线程启动起来？怎么结束线程？**

创建线程对象(继承threading.Thread)，并重写run()函数，线程的启动通过实例化线程对象并调用start()函数进行启动（多线程循环start()即可），结束线程通过join()函数。

线程池：

```python
from concurrent.futures import ThreadPoolExecutor
import time

def sayhello(a):
	if a=="a":
		time.sleep(2)
		print("hello: " + a)
	if a == "b":
		time.sleep(5)
		print("hello: " + a)
	if a == "c":
		time.sleep(6)
		print("hello: " + a)
def main():
	seed=["a","b","c"]
	start2=time.time()
	with ThreadPoolExecutor(5) as executor:   #开启最大为5的线程池
		print (1231241235141)
		executor.submit(sayhello,"a")
		print (str(time.time()).split('.')[0])
		executor.submit(sayhello, "b")
		print 324324324
	end2=time.time()
	print("time2: "+str(end2-start2))
main()
```

### **7、Python3中的range()，比如range(1,100000000)将里面的数取出来，是怎么实现这个能力的？**

Python3中的range() 返回的是一个Iterable(**可迭代对象，注意不是迭代器**)，对其调用iter(Iterable)将会得到一个迭代器，而平常我们所使用range()则表示一个范围的容器。

range(start, stop, [step]) 三个参数分别如下：

- start: 计数从 start 开始。默认是从0开始。例如range(5)等价于range(0,5)
- stop: 计数到 stop 结束，但不包括 stop。例如: range(0,5)是[0, 1, 2, 3, 4]没有5的
- step：(可有 可无)步长，默认为1。例如:range(0,5)等价于range(0, 5, 1)

### **8、静态函数，object类？**

在面向对象三大特性之一的多态中，有用到@staticmethod，这是Python的内置装饰器，用途:该函数只能访问类变量，而不能访问实例变量。

现在Python3中所有的类默认都继承object类，在Python2自定义类时不继承object类，只拥有了__doc__ , __module__和自己定义的name变量，而类的高级特性则不被继承，在实际开发中通过继承object类获得高级特性（例如继承_ _ class _ _， _ _ hash _ _ ），可以捕获异常通过__class__来定位类的名称等。

### **9、__ slot __是什么概念？什么能力？**

在定义class时通过__ slots __声明中包含若干实例变量，并为每个实例预留恰好足够的空间来保存每个变量；这样Python就不会再使用dict，从而节省内存空间。

### **10、python中数据结构有哪些？列表，字典是怎么存储的？**

列表(list)、元组(tuple)、字典(dict)、集合(set)

列表数据存储：列表是一个线性的集合，它允许用户在任何位置插入、删除、访问和替换元素。列表实现是基于数组或基于链表结构的。当使用列表迭代器的时候，双链表结构比单链表结构更快。有序的列表是元素总是按照升序或者降序排列的元素。

字典数据存储：Python 调用内部的散列函数，将键(Key)作为参数进行转换，得到一个唯一的地址(这也就解释了为什么给相同的键赋值会直接覆盖的原因，因为相同的键转换后的地址是一样滴)，然后将值(Value)存放到该地址中。

### **11、python内存多大？如何优化内存？**

Python本身内存没有限制，这和操作系统有关。

一方面可以加内存，设置更大虚拟内存；另一方面看算法（代码）是否有问题，有无数据重复；再有就是分块处理，用时间换空间；

补充：具体业务根据实际情况进行分析处理。

### **12、如何进行垃圾回收？**

Python只会在特定情况下自启动垃圾回收（分代回收），当Python运行时，会记录其中分配对象（object allocation）和取消分配对象（object deallocation）的次数。当两者的差值高于某个阈值时，垃圾回收才会启动。

可以通过 gc.get_threshold()查看阈值

通俗来讲就是当对象的引入计数为0时，就会被解释器回收。当然在交互模式下，内存不会马上释放，重新启动解释器就会释放了。

当然也可以手动垃圾回收，调用gc.collect()即可。不建议频繁使用，频繁使用会降低Python工作效率。

## **Web及Sql部分：**

### **1、谈谈对wsgi的理解？**

wsgi是描述web server与web application通信的一种规范，运行在WSGI协议之上的web框架有Flask，Django，Torando，Odoo等。

wsgi server负责从客户端接收请求，将request转发给application，将application返回的response返回给客户端。

补充：uwsgi与WSGI一样是一种通信协议是uWSGI服务器的独占协议，与wsgi协议不同是用于nginx等代理服务器通信（uWSGI是一个web服务器）

### **2、请解释一下MVT？**

MVT其实是基于MVC设计模式的，主要用于解耦。

M 表示Model，负责与数据库交互（与MVC中的M相同）

V 表示View，是核心，负责接收请求、获取数据、返回结果（与MVC中的C相同）

T 表示Template，负责呈现内容到浏览器（与MVC中的V相同）

### **3、写出Session，Cookie和Token的区别？**

Session是服务器在和客户端建立连接时添加客户端连接标志，代表服务器与客户端之间的一次会话，Session是对于服务器来讲的(保存在服务器：内存、redis | memcached缓存、数据库等)，session_id则可以保存到Cookie(客户端)中

Cookie 存储在浏览器目录中的文本文件，一旦用户从该网站或服务器退出，Cookie 可存储在用户本地的硬盘上 (俗称浏览器缓存)

Token 就是令牌，例如你授权（登录）一个程序时，它就是个依据，判断你是否已经授权该软件，提供的主要作用就是认证（针对用户）和授权（针对APP）

补充：随着移动端的崛起，无法确定request是由浏览器发送还是移动端发送，不能从传统意义上使用Session+Cookie来验证用户身份，故产生Token

### **4、请解释一下restful？说下你用过的Web框架？**

restful就是接口规范（主要是基于HTTP协议的网络服务）。

Flask、Django、Torando、Odoo等

对比：

Flask|Django是同步框架，处理Json数据速度快，用来做传统项目开发比较擅长，Flask相对比Django更加轻量级，目前使用Flask也越来越多。当然也可以做互联网项目采用Flask/Django + Celery + Redis/Memcached/RabbitMQ增加异步处理能力。

Torando是异步框架，处理远程Http请求速度快，知乎所采用web框架就是Torando。

Odoo做ERP项目的大杀器，内部封装大量ERP模板，谁用谁知道。

补充：Flask目前国内外使用率上格外突出，原因：轻量级、自由、加上异步处理不比Torando弱。

### **5、rest_framework怎么实现api接口开发？**

安装djangorestframework，做好相关配置（setting、路由等），定义序列化数据模板，在View中使用类视图继承 generics.CreateAPIView 或者函数装饰器 @api_view(['GET', 'POST']) 来实现请求控制。

详细参考官方说明文档

[Home - Django REST frameworkwww.django-rest-framework.org
  ](https://link.zhihu.com/?target=https%3A//www.django-rest-framework.org/)

### **6、django分布式？**

使用django+celery+rabbitmq实现分布式，celery做异步，rabbitmq做消息队列。

补充：不建议使用redis作为中间件，redis单线程，当业务量增大会导致很大问题，虽然有不少人依然再用。

### **7、是否了解消息队列Kafka、RocketMQ？**

Kafka是Apache旗下开源框架(Java)，高吞吐无限消息堆积，定位于日志传输，单对于核心业务交易、订单、充值等核心业务特性不足，RocketMQ是淘宝内部用重写的框架(Java)，在淘宝订单，交易，充值，流计算，消息推送，日志流式处理，binglog分发等场景。

### **8、支付实现（支付宝、银联、微信接口）？**

支付宝、银联、微信目前是没有关于Python语言的API所对应的SDK和调用示例，所以需要自己造，不过现已经有很多成熟的轮子可以使用。

支付宝支付实现（亲测可用）：

https://github.com/fzlee/alipay

微信支付实现（亲测可用）：

https://github.com/zwczou/weixin-python

银联支付实现（参考）:

[https://blog.csdn.net/qq_42571805/article/details/83313683](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_42571805/article/details/83313683)

### **9、sql语句优化、索引及自己的见解？**

推荐使用**sql server profiler**工具监控sql（分析出sql慢的相关语句：也就是执行时间过长，占用系统资源[cpu]的sql语句）

sql语句优化主要有以下4点：

**1）保证不查询多余的列与行。**

- 尽量避免select * 的存在，使用具体的列代替*，避免多余的列
- 使用where限定具体要查询的数据，避免多余的行
- 使用top，distinct关键字减少多余重复的行

**2）慎用distinct关键字**

**3）慎用union关键字**

**4）连接查询的优化**

根据业务需求获取需要的数据选择合适的连接查询，减少连接表的数据数量可以提高效率。尽量使用连接代替子查询，因为使用 join 时，MySQL 不会在内存中创建临时表。

**索引优化：**

1）较频繁的作为查询条件的字段应该创建索引；

2）唯一性太差的字段不适合单独创建索引，即使频繁作为查询条件；

3）增、删、改操作较多的数据库字段不适合建索引；

4）索引占空间的会带来存储空间的消耗，合理使用索引。

### **10、redis数据类型（结构）？**

string（字符串），hash（哈希），list（列表），set（集合）及 zset(sorted set：有序集合)

**补充：这里会延伸一个新问题，这五个数据类型（结构）分别适应于什么业务场景?**

string类型：可以直接存储序列化的对象（Json对象），也是最常用的类型；

hash类型：可以用来存储对象，这里会和string类型功能重叠，但是当对象的某个属性需要频繁修改时，不适合用string+json，每次修改都需要重新将整个对象序列化并赋值，如果使用hash类型，则可以针对某个属性单独修改，如商品的价格、销量、关注数等可以存在hash中；

list类型：lpop和rpush能实现消息队列的功能，但是不推荐，因为redis是单线程的，并且现在已经有成熟的消息队列(NIO)框架如Kafka、NSQ、RabbitMQ等；当然除了消息队列可以用来做排名（**注意：是排名不是热搜**）；

set类型：唯一的特点使得其适合用于存储好友/关注/粉丝/感兴趣的人集合（集合元素无序且不重复）

Zset类型：有序集合是对集合的一个扩展，增加了score字段。通过score字段，我们可以选出最大或者最小的topN用来做热搜榜单（微博热搜就是如此实现的）

### **11、redis、mongodb区别？及项目中的使用场景？**

1）数据结构：

redis有5种数据结构，mongodb较为单一（文档存储是使用BSON类型类似于JSON）但是支持丰富的数据表达，索引，最类似关系型数据库，支持的查询语言非常丰富，而且在4.0版本支持事务操作。

2）数据分析：

mongodb内置数据分析功能（mapreduce），而Redis不支持。

3）内存管理：

Redis 数据全部存在内存，定期写入磁盘，当内存不够时，可以选择指定的 LRU 算法删除数据。

MongoDB 数据存在内存，由 linux系统 mmap 实现，当内存不够时，只将热点数据放入内存，其他数据存在磁盘。

4）数据量及性能：

当物理内存够用的时候，redis>mongodb>mysql，

当物理内存不够用的时候，redis和mongodb都会使用虚拟内存。

实际上如果redis要开始虚拟内存，那很明显要么加内存条。

Mongodb数据存在内存，由linux系统mmap实现，当内存不够的时候，只将热点数据放入内存，其他的数据存在磁盘。所以Mongodb很吃硬盘。

**redis业务场景可以看上一题；Mongodb可以做日志服务器，甚至可以直接替代Mysql数据库直接做核心业务数据库使用。**

### **12、GET与POST区别？**

1）GET传送的数据量较小，不能大于2KB；POST传送的数据量较大，一般被默认为不受限制【这取决于服务器设置和内存大小】

2）GET安全性相对较低，一般都在地址栏可见，POST安全性相对较高，POST传递数据比较隐私，所以在地址栏看不到；但如果没有加密，他们安全级别都是一样的，随便用抓包工具都可看到

补充：GET请求把参数包含在URL中，将请求信息放在URL后面，POST请求通过request body传递参数，将请求信息放置在报文体中

3）一般而言GET用来做查询；POST主要对数据进行增删改

4）GET请求返回的内容可以被浏览器缓存起来。而每次提交的POST，浏览器在你按F5的时候会跳出确认框，浏览器不会缓存POST请求返回的内容

### **13、http与https区别？**

http的连接方式是无状态的，https则是SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议(前者端口80，后者443)。这里可能会延伸**三次握手**、**对称加密算法**【密钥】

### 14、TCP、UDP、HTTP区别？

**一**、TCP/IP代表传输控制协议/网际协议，指的是一系列协组。

　　可分为四个层次：数据链路层、网络层、传输层和应用层。

```
在网络层:有IP协议、ICMP协议、ARP协议、RARP协议和BOOTP协议。
在传输层:中有TCP协议与UDP协议。
在应用层:有FTP、HTTP、TELNET、SMTP、DNS等协议。
```

　TCP和UDP使用IP协议从一个网络传送数据包到另一个网络。把IP想像成一种高速公路，它允许其它协议在上面行驶并找到到其它电脑的出口。TCP和UDP是高速公路上的“卡车”，它们携带的货物就是像HTTP，文件传输协议FTP这样的协议等。

​    TCP和UDP是FTP，HTTP和SMTP之类使用的传输层协议。虽然TCP和UDP都是用来传输其他协议的，它们却有一个显著的不同：TCP提供有保证的数据传输，而UDP不提供。这意味着TCP有一个特殊的机制来确保数据安全的不出错的从一个端点传到另一个端点，而UDP不提供任何这样的保证。

**二.HTTP本身就是一个协议，是从Web服务器传输超文本到本地浏览器的传送协议。**

　　HTTP(超文本传输协议)是利用TCP在两台电脑(通常是Web服务器和客户端)之间传输信息的协议。客户端使用Web浏览器发起HTTP请求给Web服务器，Web服务器发送被请求的信息给客户端。

```
 HTTP协议是建立在请求/响应模型上的。首先由客户建立一条与服务器的TCP链接，并发送一个请求到服务器，请求中包含请求方法、URL、协议版本以及
相关的MIME样式的消息。服务器响应一个状态行，包含消息的协议版本、一个成功和失败码以及相关的MIME式样的消息。
    HTTP/1.0为每一次HTTP的请求/响应建立一条新的TCP链接，因此一个包含HTML内容和图片的页面将需要建立多次的短期的TCP链接。一次TCP链接的建立
将需要3次握手。
    另外，为了获得适当的传输速度，则需要TCP花费额外的回路链接时间（RTT）。每一次链接的建立需要这种经常性的开销，而其并不带有实际有用的数据
，只是保证链接的可靠性，因此HTTP/1.1提出了可持续链接的实现方法。HTTP/1.1将只建立一次TCP的链接而重复地使用它传输一系列的请求/响应消息，
因此减少了链接建立的次数和经常性的链接开销。
```

**虽然HTTP本身是一个协议，但其最终还是基于TCP的。**

**三.SOCKET：TCP/IP网络的API。**

Socket是应用层与TCP/IP协议族通信的**中间软件抽象层**，它是**一组接口**。在设计模式中，Socket其实就是一个门面模式，它把复杂的TCP/IP协议族隐藏在Socket接口后面，对用户来说，一组简单的接口就是全部，让Socket去组织数据，以符合指定的协议。

　　Socket 接口是TCP/IP网络的API，Socket接口定义了许多函数或例程，用以开发TCP/IP网络上的应用程序。

```
    这是为了实现以上的通信过程而建立成来的通信管道，其真实的代表是客户端和服务器端的一个通信进程，双方进程通过socket进行通信，而通信的规则
采用指定的协议。socket只是一种连接模式，不是协议,tcp,udp，简单的说（虽然不准确）是两个最基本的协议,很多其它协议都是基于这两个协议如，http
就是基于tcp的，用socket可以创建tcp连接，也可以创建udp连接，这意味着，用socket可以创建任何协议的连接，因为其它协议都是基于此的。
```

 

**综上所述：需要IP协议来连接网络;TCP是一种允许我们安全传输数据的机制，使用TCP协议来传输数据的HTTP是Web服务器和客户端使用的特殊协议。HTTP基于TCP协议，但是却可以使用socket去建立一个TCP连接。**

![b3ba93f400ba123c59d2297b7349e7fc.png](https://www.pianshen.com/images/164/b3ba93f400ba123c59d2297b7349e7fc.png)

## Django经典

### **1. Django的优点和缺点有哪些?**

**Django的优点**

- 功能完善、要素齐全：自带大量企业Web开发常用工具和框架（比如分页，auth，权限管理), 适合快速开发企业级网站。
- 完善的文档：经过十多年的发展和完善，Django有广泛的实践案例和完善的在线文档。开发者遇到问题时可以搜索在线文档寻求解决方案。
- 强大的数据库访问组件：Django的Model层自带数据库ORM组件，使得开发者无须学习SQL语言即可对数据库进行操作。
- Django先进的App设计理念: App是可插拔的，是不可多得的思想。不需要了，可以直接删除，对系统整体影响不大。
- 自带台管理系统admin：只需要通过简单的几行配置和代码就可以实现一个完整的后台数据管理控制平台。

**Django的缺点**

- 大包大揽: 对于一些轻量级应用不需要的功能模块Django也包括了，不如Flask轻便。
- 过度封装: 很多类和方法都封装了，直接使用比较简单，但改动起来就比较困难。
- 性能劣势: 与C, C++性能上相比，Django性能偏低，当然这是python的锅，其它python框架在流量上来后会有同样问题。
- 模板问题: django的模板实现了代码和样式完全分离，不允许模板里出现python代码，灵活度对某些程序员来说可能不够。

### **2. 说说看Django的请求生命周期**

![img](https://pic4.zhimg.com/80/v2-ed1ee3a6a6fd15b98f86bdd862c62de3_1440w.jpg)

注：最重要的是回答用户请求并不是一下子通过URL匹配就达到相应视图，返回数据也不是一下子就返回给用户，中间要经历层层中间件。这个面试题其实考的核心是中间件。

1、当用户在浏览器中输入url时,浏览器会生成请求头和请求体发给服务端

2、服务端的wsgiref模块接收用户请求并将请求进行初次封装，然后将请求交给Django的中间件

3、通过中间件之后将请求交给url,根据浏览器发送的不同url去匹配不同的视图函数

 4、视图函数根据业务逻辑调用数据库获取相应的数据，然或根据模板渲染页面

如果不涉及到数据调用，那么这个时候视图函数返回一个模板也就是一个网页给用),视图函数调用模型，模型去数据库查找数据,然后逐级返回，视图函数把返回的数据填充到模板中空格中

5、视图函数将响应的页面依次通过中间件反馈给客户端

在生产环境，你还需要很清楚地描述下图流程。

![img](https://pic2.zhimg.com/80/v2-93bd8254551b3790d246698e03d67c01_1440w.jpg)

### **3. 请列举几个Django ORM中常用的获取数据查询集(queryset)的方法**

常用方法包括filter和exclude方法。字符串模糊匹配可以使用icontains, in等多种方法。随便举几个例子即可。

### **4. 说说看Django的Queryset有哪些特性**

Django的QuerySet主要有两个特性：一是惰性的(lazy)，二是自带缓存。我们来看个例子。

下例中article_list试图从数据库查询一个标题含有django的全部文章列表。

```python
article_list = Article.objects.filter(title__contains="django")
```

但是当我们定义article_list的时候，Django的数据接口QuerySet并没有对数据库进行任何查询。无论你加多少过滤条件，Django都不会对数据库进行查询。只有当你需要对article_list做进一步运算时（比如打印出查询结果，判断是否存在，统计查询结果长度)，Django才会真正执行对数据库的查询(见下例1)。这个过程被称为queryset的执行(evaluation)。Django这样设计的本意是尽量减少对数据库的无效操作，比如查询了结果而不用是计算资源的很大浪费。

```python
# example 1
for article in article_list:
    print(article.title)
```

在例1中，当你遍历queryset(article_list)时，所有匹配的记录会从数据库获取。这些结果会载入内存并保存在queryset内置的cache中。这样如果你再次遍历或读取这个article_list时，Django就不需要重复查询了，这样也可以减少对数据库的查询。

### **5. 什么是基于函数的视图（FBV)和基于类的视图（CBV)以及各自的优点**

FBV（function base views） 就是在视图里使用函数处理请求。CBV（class base views） 就是在视图里使用类处理请求。Python是一个面向对象的编程语言，如果只用函数来开发，有很多面向对象的优点就错失了（继承、封装、多态）。所以Django在后来加入了Class-Based-View，可以让我们用类写View，这样做的优点主要下面两种：

- 提高了代码的复用性，可以使用面向对象的技术，比如Mixin（多继承）
- 可以用不同的函数针对不同的HTTP方法处理，而不是通过很多if判断，提高代码可读性

当然基于函数的视图也有自己的优点，比如对新手更友好。

### **6. 如何给基于类的视图（CBV)使用装饰器**

需要借助django.utils模块的method_decorator方法实现，它还支持decorators列表, 如下所示:

```python
from django.utils.decorators import method_decorator

decorators = [login_required, check_user_permission]


@method_decorator(decorators, name='dispatch')
class ArticleCreateView(CreateView):
    model = Article
    form_class = ArticleForm
    template_name = 'blog/article_manage_form.html'
```

### **7. 说说看使用基于类的视图(CBV)时get_queryset, get_context_data和get_object方法的作用**

**get_queryset()方法**

正如其名，该方法可以返回一个量身定制的对象列表。当我们使用Django自带的ListView展示所有对象列表时，ListView默认会返回Model.objects.all()。

```python
# Create your views here.
from django.views.generic import ListView
from .models import Article

class IndexView(ListView):

    model = Article
```

然而这可能不是我们所需要的。当我们希望只展示作者自己发表的文章列表且按文章发布时间逆序排列时，我们就可以通过更具体的get_queryset方法来返回一个我们想要显示的对象列表。

```python
# Create your views here.
from django.views.generic import ListView
from .models import Article
from django.utils import timezone

class IndexView(ListView):

    template_name = 'blog/article_list.html'
    context_object_name = 'latest_articles'

    def get_queryset(self):
        return Article.objects.filter(author = self.request.user).order_by('-pub_date')
```

**get_context_data()**

get_context_data可以用于给模板传递模型以外的内容或参数，非常有用。例如现在的时间并不属于Article模型。如果你想把现在的时间传递给模板，你还可以通过重写get_context_data方法（如下图所示)。因为调用了父类的方法，

```text
# Create your views here.
from django.views.generic import ListView
from .models import Article
from django.utils import timezone

class IndexView(ListView):

    queryset = Article.objects.all().order_by("-pub_date")
    template_name = 'blog/article_list.html'
    context_object_name = 'latest_articles'

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context['now'] = timezone.now() #只有这行代码有用
        return context
        
```

**get_object()方法**

DetailView和EditView都是从URL根据pk或其它参数调取一个对象来进行后续操作。下面代码通过DetailView展示一篇文章的详细信息。

```python
# Create your views here.
from django.views.generic import DetailView
from django.http import Http404
from .models import Article
from django.utils import timezone

class ArticleDetailView(DetailView):

    queryset = Article.objects.all().order_by("-pub_date") #等同于model = Article
    template_name = 'blog/article_detail.html'
    context_object_name = 'article'
```

然而上述代码可能满足不了你的需求。比如你希望一个用户只能查看或编辑自己发表的文章对象。当用户查看别人的对象时，返回http 404错误。这时候你可以通过更具体的get_object()方法来返回一个更具体的对象。代码如下:

```python
from django.views.generic import DetailView
from django.http import Http404
from .models import Article
from django.utils import timezone

class ArticleDetailView(DetailView):

    queryset = Article.objects.all().order_by("-pub_date")
    template_name = 'blog/article_detail.html'
    context_object_name = 'article'

    def get_object(self, queryset=None):
        obj = super().get_object(queryset=queryset)
        if obj.author != self.request.user:
            raise Http404()
        return obj
```

### **8. 你能列举几个减少数据库查询次数的方法吗？**

- 利用Django queryset的惰性和自带缓存的特性
- 使用select_related和prefetch_related方法在数据库层面进行Join操作
- 使用缓存

### **9. Django的模型继承有哪几种方式? 它们有什么区别以及何时使用它们?**

Django的模型继承有如下3种方式:

1. 抽象模型继承(abstract model)

2. 多表模型继承(multi-table inheritance)

3. 代理模型(proxy model)

它们的区别如下:

- Django不会为抽象模型在数据库中生成自己的数据表。父类Meta中的abstract=True也不会传递给子类。如果你发现多模型有很多共同字段时，需使用抽象模型继承。
- 多表模型继承与抽象模型继承最大的区别在于Django也会为父类模型建立自己的数据表，同时隐式地在父类和子类之间建立一个一对一关系。
- 如果我们只想改变某个模型的行为方法，而不是添加额外的字段或创建额外的数据表，我们就可以使用代理模型(proxy model)。设置一个代理模型，需要在子类模型Meta选项中设置proxy=True， Django不会为代理模型生成新的数据表。

### **10. 说说看如何自定义模型标签(templatetags)和过滤器(filter)?**

首先你要在你的app目录下新建一个叫templatetags的文件夹(不能取其它名字), 里面必需包含__init__.py的空文件。在该目录下你还要新建一个python文件专门存放你自定义的模板标签函数，本例中为blog_extras.py，当然你也可以取其它名字。整个目录结构如下所示:

```python
blog/
   __init__.py
   models.py
   templatetags/
       __init__.py
       blog_extras.py
   views.py
```

在模板中使用自定义的模板标签时，需要先使用{% load blog_extras %}载入自定义的过滤器，然后通过{% tag_name %} 使用它。

举例

我们将定义3个简单模板标签，一个返回string, 一个给模板context传递变量，一个显示渲染过的模板。我们在blog_extra.py里添加下面代码。

\#blog_extra.py

```python
from django import template
import datetime
from blog.models import Article

register = template.Library()

# use simple tag to show string
@register.simple_tag
def total_articles():
    return Article.objects.filter(status='p').count()

# use simple tag to set context variable
@register.simple_tag
def get_first_article():
    return Article.objects.filter(status='p').order_by('-pub_date')[0]

# show rendered template
@register.inclusion_tag('blog/latest_article_list.html')
def show_latest_articles(count=5):
    latest_articles = Article.objects.filter(status='p').order_by('-pub_date')[:count]
    return {'latest_articles': latest_articles, }
```

### **11. 简单说说看 Django的CSRF防御机制**

Django的CSRF保护主要是通过`django.middleware.csrf.CsrfViewMiddleware`中间件来实现的。主要流程如下:

- Django 第一次响应来自某个客户端的get请求时，会在服务器端随机生成一个 csrftoken(一串64位的随机字符串)，把这个 token 放请求头的 cookie 里返回给用户。
- 所有通过POST方式提交的表单在渲染时中必须包含一个 csrfmiddlewaretoken 隐藏字段 （在模板中通过{% csrf_token %}标签生成)。
- 当用户通过POST提交表单时，Django会从请求头cookie取`csrftoken`这一项的值，再从POST表单里取`csrfmiddlewaretoken`交由中间件进行校验两者是否一致。如果一致表明这是一个合法请求，否则返回403 Forbidden.

注意`csrftoken`和`csrfmiddlewaretoken`并不是简单相等的两个字符串，而是通过算法判断是否一致相等的，如下图所示。

![img](https://pic4.zhimg.com/80/v2-34e2c87ab0bee987c738f67f4233843b_1440w.jpg)

### **12. Django中使用AJAX发送POST请求时如何通过CSRF认证？**

```html
1. 第一种方式直接在发送数据中加入csrfmiddlewaretoken
<script>
  $("#btn").on("click",function () {
        $.ajax({
            url:"/some_url/",
            type:"POST",
            data:{
                csrfmiddlewaretoken:{{ csrf_token }}, //写在模板中，才会被渲染
            },
            success:function (data) {
            }
        })
    })
</script>

2.通过jquery选择器获取csrfmiddlewaretoken
<script>
  $("#btn").on("click",function () {
        $.ajax({
            url:"/some_url/",
            type:"POST",
            data:{
                csrfmiddlewaretoken:$('[name="csrfmiddlewaretoken"]').val(),
            },
            success:function (data) {
            }
        })
    })
</script>

3. 使用jquery.cookie.js调用请求头cookie中的csrftoken
<script src="/static/jquery.cookie.js"></script> //必须先引入它
<script>
    $("#btn").on("click",function () {
     $.ajax({
        url:"/some_url/",
        type:"POST",
        headers:{"X-CSRFToken":$.cookie('csrftoken')},
        data:$("#f1").serialize()
    }
    )
   })
</script>
```

#### **13. 什么情况下需要使用select_related和prefetch_related方法以及两者的区别**

当你查询单个主对象或主对象列表并需要在模板或其它地方中使用到每个对象的关联对象信息
时，请一定记住使用select_related和prefetch_related一次性获取所有对象信息，从而提升

数据库查询效率，避免重复查询。两个方法都是Django ORM优化数据查询必须要熟练掌握
的方法。

两者的区别是：

- 对单对单(OneToOne)或单对多外键(ForeignKey)字段，使用select_related方法
- 对于多对多字段(ManyToMany)和反向外键关系，使用prefetch_related方法
- select_related方法执行一次数据库查询，prefetch_related方法执行两次数据库查询
- 使用Prefetch方法可以给prefetch_related方法额外添加额外条件和属性。

### **14. 如何从数据表中获取一个随机对象？**

可以使用order_by('?').first()随机获取一个对象

```python
def get_random_object():
    return MyModel.objects.order_by("?").first()
```

15. ### 说说看aggregate和annotate方法的作用并举几个例子

    aggregate的中文意思是聚合, 源于SQL的聚合函数。Django的aggregate()方法作用是对一组值(比如queryset的某个字段)进行统计计算，并以字典(Dict)格式返回统计计算结果。
    django的aggregate方法支持的聚合操作有AVG / COUNT / MAX / MIN /SUM 等。

```python
Student.objects.aggregate(Avg('age‘), Max('age‘), Min('age‘))
# 同时获取学生年龄均值, 最大值和最小值, 返回字典
{ 'age__avg': 12, 'age__max': 18, 'age__min': 6, }

Hobby.objects.aggregate(Max('student__age'))
# 根据Hobby反查学生最大年龄。查询字段student和age间有双下划线哦。
{ 'student__age__max': 12 }
```


annotate的中文意思是注释，一个更好的理解是分组(Group By)。如果你想要对数据集先进行分组然后再进行某些聚合操作或排序时，需要使用annotate方法来实现。与aggregate方法不同的是，annotate方法返回结果的不仅仅是含有统计结果的一个字典，而是包含有新增统计字段的查询集(queryset）.

```python
# 按学生分组，统计每个学生爱好数量，并自定义字段名
Student.objects.annotate(hobby_count_by_student=Count('hobbies'))

# 按爱好分组，再统计每组学生数量。
Hobby.objects.annotate(Count('student'))

# 按爱好分组，再统计每组学生最大年龄。
Hobby.objects.annotate(Max('student__age'))

# 先按爱好分组，再统计每组学生数量, 然后筛选出学生数量大于1的爱好。
Hobby.objects.annotate(student_num=Count('student')).filter(student_num__gt=1)

# 先按爱好分组，筛选出以'd'开头的爱好，再统计每组学生数量。
Hobby.objects.filter(name__startswith="d").annotate(student_num=Count('student‘))
```

### **16. Django中如何使用redis做缓存?**

安装好redis后，你需要安装django-redis才能在django中使用redis。django-redis安装命令如下:

```text
pip install django-redis
```


settings.py中加入以下内容配置缓存。your_host_ip换成你的服务器地址,yourpassword换成你的服务器登陆密码。

```text
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://your_host_ip:6379',
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
             "PASSWORD": "yourpassword",
        },
    },
}
```


你还可以在settings.py设置缓存默认过期时间（非必须)
REDIS_TIMEOUT=7*24*60*60

### **17. Django项目上传到代码库时是否需要忽略数据库迁移文件?**

数据库迁移文件位于每个app文件夹的migrations文件夹里，这些文件记录了模型的创建与改
动。每次当你创建模型或对模型字段进行修改，然后运行python manage.py

makemigrations命令时都会有新的迁移文件产生。Django官方文档特别说明这些迁移文件
属于Django项目代码中很重要的一部分，不应删除或忽略，所以建议上传。

### **18. 如何在模板中获取当前访问url地址**

在模板中你可以使用{{ request.path }}获取当前url，如果要获取带querystring的完整url你可以使用{{ request.get_full_path }}。如果你要获取完整绝对路径，你可以使用 {{ request.build_absolute_uri }}。

如果一个URL如下所示，则利用不同方法可以得到不同url
[https://www.baidu.com/search/?keyword=django](https://link.zhihu.com/?target=https%3A//www.baidu.com/search/%3Fkeyword%3Ddjango)

- request.path: /search/
- request.get_full_path: /search/?keyword=django
- request.build_absolute_uri: [https://www.baidu.com/search/?keyword=django](https://link.zhihu.com/?target=https%3A//www.baidu.com/search/%3Fkeyword%3Ddjango)

### **19. 使用F方法更新一个对象或多个对象的某个字段有什么优点?**

通常情况下我们在更新数据时需要先从数据库里将原数据取出后放在内存里，然后编辑某些字段或属性，最后提交更新数据库。使用F方法则可以帮助我们避免将所有数据先载入内存，而是直接生成SQL语句更新数据库。

假如我们需要对所有产品的价格涨20%，我们通常做法如下。当产品很少的时候，对网站性能没影响。但如果产品数量非常多，把它们信息全部先载入内存会造成很大性能浪费。

```text
products = Product.objects.all()
for product in products:
    product.price *= 1.2
    product.save()
```


使用F方法可以解决上述问题。我们直接可以更新数据库，而不必将所有产品载入内存。

```text
from django.db.models import F

Product.objects.update(price=F('price') * 1.2)
```


我们也可以使用F方法更新单个对象的字段，如下所示：

```python3
product = Product.objects.get(pk=5009)
product.price = F('price') * 1.2
product.save()
```


但值得注意的是当你使用F方法对某个对象字段进行更新后，需要使用refresh_from_db()方法后才能获取最新的字段信息（非常重要！)。如下所示：

```text
product.price = F('price') + 1
product.save()
print(product.price)            # <CombinedExpression: F(price) + Value(1)>
product.refresh_from_db()
print(product.price)            # Decimal('13.00')
```



### **20. 说说 nginx 和 uWISG 服务器之间如何配合工作的？**

- 首先浏览器发起 http 请求到 nginx 服务器，Nginx 根据接收到请求包，进行 url 分析,
  判断访问的资源类型。如果是静态资源，直接读取静态资源返回给浏览器。
- 如果请求的是动态资源就转交给 uwsgi服务器。
- uwsgi 服务器根据自身的uwsgi 和 WSGI 协议，找到对应的 Django 框架。
- Django 框架下的应用进行逻辑处理后，将返回值发送到 uwsgi 服务器。
- uwsgi 服务器再返回给 nginx，最后 nginx将返回值返回给浏览器进行渲染显示给用户。

### **21. 说说看Django信号(Signals)的工作原理, 主要应用场景及内置信号**

Django 提供一个了“信号分发器”机制，允许解耦的应用在框架的其它地方发生操作时会被通知到。 通俗而讲Django信号的工作原理就是当某个事件发生的时候会发出一个信号(signals), 而监听这个信号的函数(receivers)就会立即执行。Django信号的应用场景很多，尤其是用于不同模型或程序间的联动。常见例子包括创建User对象实例时创建一对一关系的UserProfile对象实例，或者每当用户下订单时触发给管理员发邮件的动作。

Django内置信号包括：

- django.db.models.signals.pre_save & post_save在模型调用 save()方法之前或之后发送。
- django.db.models.signals.pre_init& post_init在模型调用_init_方法之前或之后发送。
- django.db.models.signals.pre_delete & post_delete在模型调用delete()方法或查询集调用delete() 方法之前或之后发送。
- django.db.models.signals.m2m_changed在模型多对多关系改变后发送。
- django.core.signals.request_started & request_finished Django建立或关闭HTTP 请求时发送。

### **22. 什么是中间件(middleware)，中间件(middleware)的应用场景**

*中间件(Middleware)是一个镶嵌到django的request/response处理机制中的一个钩子(hooks) 框架。它是一个可以修改django全局输入或输出的一个底层插件系统。*

一个请求HttpRequest在传递给视图View处理前要经过中间件处理，经过View处理后的响应也要经过中间件处理才能返回给用户。我们可以编写自己的中间件实现权限校验，限制用户请求、打印日志、改变输出内容等多种应用场景，比如：

- 禁止特定IP地址的用户或未登录的用户访问我们的View视图函数
- 对同一IP地址单位时间内发送的请求数量做出限制
- 在View视图函数执行前记录用户的IP地址
- 在View视图函数执行前传递额外的变量或参数
- 在View视图函数执行前或执行后把特定信息打印到log日志
- 在View视图函数执行后对reponse数据进行修改后返回给用户

### **23. Django有哪些内置中间件及每个中间件的作用**

Django的settings.py里已经注册了一些自带的中间件，每个中间件都负责一个特定的功能。SecurityMiddleware：为request/response提供了几种安全改进，无它不安全

- SessionMiddleware：开启session会话支持，无它无session
- CommonMiddleware：基于APPEND_SLASH和PREPEND_WWW的设置来重写URL，如果APPEND_SLASH设为True，并且初始URL 没有以斜线结尾以及在URLconf 中没找到对应定义，这时形成一个斜线结尾的新URL；如果PREPEND_WWW设为True，前面缺少 www.的url将会被重定向到相同但是以一个www.开头的url。
- CsrfViewMiddleware：添加跨站点请求伪造的保护，通过向POST表单添加一个隐藏的表单字段，并检查请求中是否有正确的值，无它无csrf保护
- AuthenticationMiddleware：在视图函数执行前向每个接收到的user对象添加HttpRequest属性，表示当前登录的用户，无它用不了request.user
- MessageMiddleware：开启基于Cookie和会话的消息支持，无它无message
- XFrameOptionsMiddleware：对点击劫持的保护

### **24. Django项目中什么时候使用中间件，什么时候使用装饰器？**

中间件和装饰器均广泛用于权限校验，缓存和日志。中间件对Django的输入或输出的改变是全局的，而装饰器一般只改变单个视图的输入输出。如果让你希望对Django的输入或输出做出全局性的改变时，需要使用中间件，否则使用装饰器。

举个例子，我们在[装饰器](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMjM5OTMyODA4Nw%3D%3D%26mid%3D2247484237%26idx%3D1%26sn%3Dc2d050ff33d2957e8971034476cd8326%26chksm%3Da73c6375904bea639b58f93cb82e714e8ae339f195818179ad9cb15e1413a2b63fcd1f30039f%26scene%3D21%23wechat_redirect)一文中介绍了如何使用@login_required装饰器要求用户必须先登录才能访问我们的某个视图函数。试想我们有个网站绝大部分视图函数都需要用户登录，每个视图函数前面都需要加上@login_required装饰器是比较傻的行为。借助于中间件，我们无需使用装饰器即可全局实现：只有登录用户才能访问视图函数，匿名用户跳转到登录页面。实现原理也很简单，在一个request到达视图函数前，我们先对request.user是否验证通过进行判断，然后再进行跳转。

### **25. 使用Celery执行异步任务时如何给任务设置超时时间?**

如果希望全局性地设置每个任务的超时时间，可以使用如下设置：

```text
CELERYD_TASK_TIME_LIMIT = 10*60 #10分钟
```

但如果你的项目中有多个任务，但每个任务执行的周期和执行的耗时都不一样，可以使用@task装饰器的time_limit或soft_time_limit属性来给每个任务设置超时时间。两者区别是：

- time_limit参数超时直接kill掉当前worker；
- soft_time_limit参数超时时会报错而不会kill当前worker，可以捕获。

```python
@app.task(name='tasks.main', bind=True, soft_time_limit=2, errback=error_fun)
def task_main(self, param):
    try:
        time.sleep(10)
    except SoftTimeLimitExceeded as e:
        print e
```

### **26. 什么情况下需要自定义context_processors(上下文处理器)**

当你需要一个视图函数或模板提供或设置全局变量时，你需要使用context_processors(上下文处理器)。我们在视图和模板中可以随意使用request这个对象作为变量，不需要额外传递，就是因为django.core.context_processors.request把request变成了一个全局变量。

context_processors(上下文处理器)在很多场景下非常有用，举个实际点的例子。一个博客的每篇文章详情上都会有标签云，文章归档，友情链接等信息，这些信息每篇文章都是可以公用的信息。如果每一篇文章的DetailView都从数据库查询相关数据再返回给前端模板，这就造成了数据库查询的浪费，增加了服务器的负担。如果把这些数据通过context_processors设置成全局变量，那么所有的视图和模板都能够直接访问不需要再重复查询数据库了，是不是很帅？

### **27. Django如何生成静态html文件**

使用render_to_string方法生成content，然后写入html文件。

```python
from django.shortcuts import render
from django.template.loader import render_to_string
import os
 
 
def my_view(request):
    context = {'some_key': 'some_value'}
 
    static_html = '/path/to/static.html'
 
    if not os.path.exists(static_html):
        content = render_to_string('template.html', context)
        with open(static_html, 'w') as static_file:
            static_file.write(content)
 
    return render(request, static_html)
```

### **28. Django项目如何实现高并发？**

可以从如下几个角度讲:

- 使用nginx进行反向代理和负载均衡
- 数据库分库和读写分离(含主从复制)
- 使用nosql数据库比如redis缓存热点数据
- 耗时任务（比如发邮件或写入操作)交由celery异步处理
- 使用Gzip或django-compressor压缩静态文件
- 使用CDN加速静态文件访问

### **29. 什么是wsgi,uwsgi,uWSGI？**

WSGI (Web Server Gateway Interface)

Web服务器网关接口,是一套协议。用于接收用户请求并将请求进行初次封装，然后将请求交给web框架。实现wsgi协议的模块有：

1.wsgiref,本质上就是编写一个socket服务端，用于接收用户请求(django)

2.werkzeug,本质上就是编写一个socket服务端，用于接收用户请求(flask)

uwsgi:

与WSGI一样是一种通信协议，它是uWSGI服务器的独占协议,用于定义传输信息的类型

uWSGI:

是一个web服务器,实现了WSGI协议,uWSGI协议,http协议,

**0. 列举5个常用的Django第三方库**

答案不限于：

- [第三方社交登录: django-allauth](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMjM5OTMyODA4Nw%3D%3D%26mid%3D2247483857%26idx%3D1%26sn%3D848e9778942008b5f2a170d44d28bc62%26chksm%3Da73c61e9904be8ffa7facea412fffc94e4e6372c702f1cf0cda8d65cd1e0c66ffb23f914998d%26scene%3D21%23wechat_redirect)
- 过滤器: django-filter
- 富文本编辑器：[ckeditor](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMjM5OTMyODA4Nw%3D%3D%26mid%3D2247483721%26idx%3D1%26sn%3Daed2d8ab000953bf9ccc2b7a538f07f9%26chksm%3Da73c6171904be867ede371a0d7dd963c27bf765d85db738aa591ce678b025566282ecb4e6b57%26scene%3D21%23wechat_redirect)
- 调试debug工具: [django-debug-toolbar](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMjM5OTMyODA4Nw%3D%3D%26mid%3D2247484306%26idx%3D1%26sn%3D48a89cdf74edff43d727c61ac14b480a%26chksm%3Da73c63aa904beabc1975f9e38bafb36ac9fff041b0a468fa66c82279b03c1fdbf9364731e0c6%26scene%3D21%23wechat_redirect)
- 快速生成可以用于生产环境的项目目录：[cookiecutter](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMjM5OTMyODA4Nw%3D%3D%26mid%3D2247484396%26idx%3D1%26sn%3Df7bb8b9790f66217e038dbfb1d0c4925%26chksm%3Da73c63d4904beac2d3edb6f52ea532b739e6550afcdc6822fe913063fb38e3f4d683eeaccffb%26scene%3D21%23wechat_redirect)
- API工具：django rest framework