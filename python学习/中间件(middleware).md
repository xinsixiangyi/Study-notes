## 中间件(middleware)的工作原理和应用场景举例

在初级Django开发项目中，你大概率用不到中间件(Middleware)。但随着项目需求越来越复杂，你就需要开始编写自己的中间件了。当你了解到Django中间件(middleware)的工作原理和作用之后，你就会知道中间件的灵活和强大之处了。本文着重分析中间件(middleware)的工作原理和应用场景，并提供一些使用自定义中间件的简单例子。注意：本文示例所使用Django版本为Django 3.0。



**什么是中间件(middleware)及\**中间件(middleware)\**的工作原理**

*中间件(Middleware)是一个镶嵌到django的request/response处理机制中的一个钩子(hooks) 框架。它是一个可以修改django全局输入或输出的一个底层插件系统。*

上面这段是Django官方文档中对于Middleware的介绍，我们首先要了解下Django的request/response处理机制，然后再看看Middleware在整个处理机制中的角色及其工作原理。

HTTP Web服务器工作原理一般都是接收用户发来的请求(request), 然后给出响应(response)。Django也不例外，其一般工作方式是接收request对象和其它参数，交由视图(view)处理，然后给出它的响应(respone)数据: 渲染过的html文件或json格式的数据。然而在实际工作中Django并不是接收到request对象后，马上交给视图函数或类(view)处理，也不是在view执行后立马给用户返回reponse。事实上Django最初接收的是HttpRequest对象，而不是request对象，正是中间件的作用把HttpRequest对象和user对象打包成了一个全局变量request对象，这样你才可以View中使用request作为变量或者在模板中随意调用request.user。

中间件(Middleware)在整个Django的request/response处理机制中的角色如下所示：

<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>HttpRequest -> Middleware -> View -> Middleware -> HttpResponse</span>

正是由于一个请求HttpRequest在传递给视图View处理前要经过中间件处理，经过View处理后的响应也要经过中间件处理才能返回给用户，我们可以编写自己的中间件实现权限校验，限制用户请求、打印日志、改变输出内容等多种应用场景，比如：

- 禁止特定IP地址的用户或未登录的用户访问我们的View视图函数
- 对同一IP地址单位时间内发送的请求数量做出限制
- 在View视图函数执行前记录用户的IP地址
- 在View视图函数执行前传递额外的变量或参数
- 在View视图函数执行前或执行后把特定信息打印到log日志
- 在View视图函数执行后对reponse数据进行修改后返回给用户

值得一提的是中间件对Django的输入或输出的改变是全局的，反之亦然。如果让你希望对Django的输入或输出做出全局性的改变时，需要使用中间件。举个例子，我们在[装饰器](http://mp.weixin.qq.com/s?__biz=MjM5OTMyODA4Nw==&mid=2247484237&idx=1&sn=c2d050ff33d2957e8971034476cd8326&chksm=a73c6375904bea639b58f93cb82e714e8ae339f195818179ad9cb15e1413a2b63fcd1f30039f&scene=21#wechat_redirect)一文中介绍了如何使用@login_required装饰器要求用户必须先登录才能访问我们的视图函数。试想我们有个网站绝大部分视图函数都需要用户登录，每个视图函数前面都需要加上@login_required装饰器是比较傻的行为。借助于中间件，我们无需使用装饰器即可全局实现：只有登录用户才能访问视图函数，匿名用户跳转到登录页面。实现原理也很简单，在一个request到达视图函数前，我们先对request.user是否验证通过进行判断，然后再进行跳转。另外Django对POST表单中携带的CSRF token的全局校验也是通过CsrfViewMiddleware这个中间件进行的，而不是通过单个装饰器实现的。



**Django自带中间件介绍**

当你创建一个新django项目时，你会发现settings.py里已经注册了一些Django自带的中间件，每个中间件都负责一个特定的功能。

```python
MIDDLEWARE = ['django.middleware.security.SecurityMiddleware',    'django.contrib.sessions.middleware.SessionMiddleware',    'django.middleware.common.CommonMiddleware',    'django.middleware.csrf.CsrfViewMiddleware',    'django.contrib.auth.middleware.AuthenticationMiddleware',    'django.contrib.messages.middleware.MessageMiddleware',    'django.middleware.clickjacking.XFrameOptionsMiddleware',]
```

```
每个中间件的功能如下:
```

- SecurityMiddleware：为request/response提供了几种安全改进，无它不安全
- SessionMiddleware：开启session会话支持，无它无session
- CommonMiddleware：基于APPEND_SLASH和PREPEND_WWW的设置来重写URL，如果APPEND_SLASH设为True，并且初始URL 没有以斜线结尾以及在URLconf 中没找到对应定义，这时形成一个斜线结尾的新URL；如果PREPEND_WWW设为True，前面缺少 www.的url将会被重定向到相同但是以一个www.开头的url。
- CsrfViewMiddleware：添加跨站点请求伪造的保护，通过向POST表单添加一个隐藏的表单字段，并检查请求中是否有正确的值，无它无csrf保护
- AuthenticationMiddleware：在视图函数执行前向每个接收到的user对象添加HttpRequest属性，表示当前登录的用户，无它用不了request.user
- MessageMiddleware：开启基于Cookie和会话的消息支持，无它无message
- XFrameOptionsMiddleware：对点击劫持的保护

如果你要实现全站缓存, 还需要使用UpdateCacheMiddleware和FetchFromCacheMiddleware，但一定要注意它们的顺序，Update在前和Fetch在后。

```python
MIDDLEWARE = [ 
 'django.middleware.cache.UpdateCacheMiddleware',    'django.middleware.common.CommonMiddleware',    'django.middleware.cache.FetchFromCacheMiddleware',
]
```



除此以外Django还提供了压缩网站内容的**GZipMiddleware，**根据用户请求语言返回不同内容的**LocaleMiddleware**和给GET请求附加条件的**ConditionalGetMiddleware。**这些中间件都是可选的。



注意：从Django 1.10起, settings.py里注册中间件使用MIDDLEWARE=，而不是MIDDLEWARE_CLASSES= 。





**Django的中间件执行顺序**



当你在settings.py注册中间件时一定要要考虑中间件的执行顺序，中间件在request到达view之前是从上向下执行的，在view执行完后返回reponse过程中是从下向上执行的，如下图所示。举个例子，如果你自定义的中间件有依赖于request.user（比如判断用户是否登录)，那么你自定义的中间件一定要放在AuthenticationMiddleware的后面。

![img](https://mmbiz.qpic.cn/mmbiz_png/buaFLFKicRoC9GzBeibAq1UPJ0Zl3vlwDias6tHa7lyiaJiafZWOnfl1fOQPMObvJwTXAzkXtR2gjPYoBfhGIFDX1bg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

### **自定义中间件**

###### 自定义中间件你首先要在app所属目录下新建一个文件middleware.py, 添加好编写的中间件代码，然后在项目settings.py中把它添加到MIDDLEWARE列表进行注册，添加时一定要注意顺序。Django提供了两种编写自定义中间件的方式：函数和类，基本框架如下所示:



### 函数实现方式

```python
def simple_middleware(get_response):
# One-time configuration and initialization. 一次性设置和初始化
    def middleware(request):
        # Code to be executed for each request before
# the view (and later middleware) are called.        # request请求到达视图函数执行前的代码

        response = get_response(request)

        # Code to be executed for each request/response after
        # the view is called. 视图函数执行后的代码

        return response

    return middleware
```

### 类实现方式

```python
class SimpleMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response
        # One-time configuration and initialization.一次性设置和初始化

    def __call__(self, request):
        # Code to be executed for each request before
		# the view (and later middleware) are called.        # 视图函数执行前的代码
        response = self.get_response(request)

        # Code to be executed for each request/response after
        # the view is called. 视图函数执行后的代码

        return response
```

我们来看下面一个简单的例子。小编我习惯了用类来编写中间件，所以这里用类来演示。我们编写了一个名为MyFirstMiddleware中间件，介入到了Django的request/response整个处理过程，打印出请求执行过程，并在视图函数前打印出用户是否登录。

###  *#users/middleware.py*

```python
class MyFirstMiddleware:    
    def __init__(self, get_response):        
        self.get_response = get_response        
        # One-time configuration and initialization.一次性设置和初始化
    def __call__(self, request):        
        # Code to be executed for each request before        
        # the view (and later middleware) are called.        
        print("接收到request请求，视图函数马上执行")        
        if not request.user.is_authenticated:            
            print("该请求用户尚未登录")
        response = self.get_response(request)        
        # Code to be executed for each request/response after        
        # the view is called. 视图函数执行后的代码        
        print("视图函数执行结束，准备提供响应")
        return response
```

```python
#settings.py里注册。最后一个中间件是自定义的, app名为users
MIDDLEWARE = [    'django.middleware.security.SecurityMiddleware',    'django.contrib.sessions.middleware.SessionMiddleware',    'django.middleware.common.CommonMiddleware',    'django.middleware.csrf.CsrfViewMiddleware',    'django.contrib.auth.middleware.AuthenticationMiddleware',    'django.contrib.messages.middleware.MessageMiddleware',    'django.middleware.clickjacking.XFrameOptionsMiddleware',    'users.middleware.MyFirstMiddleware',]
```

因为我们自定义的中间件依赖于request对象，我们一定要放在AuthenticationMiddleware的后面。注册好中间件后，如果你运行python manage.py runserver 你就会看到如下输出:

![img](https://mmbiz.qpic.cn/mmbiz_png/buaFLFKicRoAzS8r6kjoeiabhlaoDjszrjb1Mk3HAl5ZoY1b6YJeOKVowDOOohcBlibzQIRhs4tFkpiaz1yia9k2vLg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



如果本篇文章你都看懂了并重复了本文代码，那么恭喜你终于学会编写自己的Django中间件啦。注意：本文代码是基于Django 3.0版本的，如果你的django版本是1.x或2.x版本的，你需要按如下方式自定义中间件，并使用MIDDLEWARE_CLASSES注册。

```python
from django.utils.deprecation import MiddlewareMixin 
class MyMiddleware(MiddlewareMixin):    
    def process_request(self, request):        
        print("Request before view is called!")
    def process_response(self, request, response):        
        print("Response after view is called!")        
        return response
    def process_exception(self, request, exception):        
        print("Exception!")
```