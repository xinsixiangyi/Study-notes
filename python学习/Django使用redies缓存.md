## Django中如何使用Redis进行缓存详细教程（含Windows系统下安装redis)



对于非经常更新的服务器数据，若每次都从硬盘读取一次，会浪费服务器资源、拖慢响应速度，而且数据更新频率较高，服务器负担比较大。若保存到数据库，还需要额外建立一张对应的表存储数据。一个更好的方法是在Django中使用Redis进行缓存。本文转载于**Yrish**和**宅神kin**的两篇优秀博文，详细讲解了Redis的安装与配置，并介绍了如何在Django中使用redis进行缓存。



**redis的安装**

(1)在Windows系统中安装redis

Redis不支持Windows！在它官网写得很清楚。但是开发环境一般是Windows系统。为了方便开发和调试，需要在Windows中安装Redis。微软自己弄了Redis的Windows版本。打开https://github.com/MSOpenTech/redis/releases下载msi安装包。该版本是64位。安装msi过程中，有个选项是否加入系统环境变量，记得勾上。一路下一步，安装。完成之后打开cmd，输入redis-server命令查看是否可以使用，不可以则重启一下即可。直接输入redis-server命令使用的配置文件是安装目录下的redis.windows.conf文件。 若提示错误 “ConnectionError: Error 10061 connecting to None:6379”，可打开cmd连续输入如下命令：



```
redis-cli shutdown

redis-server
```



(2)在ubuntu下安装

```
sudo apt-get install redis-server
```

安装完成后，Redis服务器会自动启动。输入redis-cli可进入命令行。



```
root@VM-60-191-ubuntu:~redis-cli
```

\#查看帮助

```
127.0.0.1:6379> help  

redis-cli 3.0.6


```

 \#设置k-v记录

```
127.0.0.1:6379> set key1 "helloword"

OK
```

\#根据键查找记录

```
127.0.0.1:6379> get key1

"helloword"
```



**设置\**redis\**密码与配置**

默认情况下，访问Redis服务器是不需要密码的，为了让其他服务器使用同增加安全性我们需要设置Redis服务器的访问密码。设置访问密码为yourpassword。 



由于redis默认绑定本机的，所以第一步取消该设置：

```
sudo vim /etc/redis/redis.conf
```



用vim打开该配置文件，然后注释掉下面这行：

```
#bind 127.0.0.1
```



然后设置登录密码，用vim打开配置文件，配置文件较长，命令模式下输入/requirepass foobared快速搜索该配置项：



\#编辑配置文件

```
sudo vim /etc/redis/redis.conf
```



\#找到下面这一行并去除注释（可以搜索requirepass)我的 是396行

\#requirepass foobared 未修改之前



\#修改成

```
requirepass 941020 #假设941020是我的redis密码
```



修改后重启服务器使配置生效：

```
root@VM-60-191-ubuntu:~sudo /etc/init.d/redis-server restart

[ ok ] Restarting redis-server (via systemctl): redis-server.service.
```



此时在登录redis,如无密码，权限被控制:

root@VM-60-191-ubuntu:~redis-cli

127.0.0.1:6379> keys *

(error) NOAUTH Authentication required.



用密码登录，具有权限

```
root@VM-60-191-ubuntu:~redis-cli -a 941020

127.0.0.1:6379> keys *

 "key2"
```



通过以下命令从另一台linux服务器访问redis, password替换为你的密码，hostip替换为要访问的服务器ip地址。

redis-cli -a password -h hostip



**安装django-redis和settings.py配置**

安装好redis后，你需要安装django-redis才能在django中使用redis。django-redis安装命令如下:

pip install django-redis



settings.py中加入以下内容配置缓存。your_host_ip换成你的服务器地址,yourpassword换成你的服务器登陆密码。

```
CACHES = {

  'default': {

​    'BACKEND': 'django_redis.cache.RedisCache',

​    'LOCATION': 'redis://your_host_ip:6379',

​    "OPTIONS": {

​      "CLIENT_CLASS": "django_redis.client.DefaultClient",

​       "PASSWORD": "yourpassword",

​    },

  },

}
```



你还可以在settings.py设置缓存默认过期时间（非必须)

```
REDIS_TIMEOUT=7*24*60*60

CUBES_REDIS_TIMEOUT=60*60

NEVER_REDIS_TIMEOUT=365*24*60*60
```



**测试django缓存是否成功**

本步骤非必须，只是为了测试看可否正常使用redis。 

进入django的后台命令模式：

python manage.py shell



逐条输入如下命令测试：

from django.core.cache import cache  #引入缓存模块

cache.set('v', '555', 60*60)   #写入key为v，值为555的缓存，有效期30分钟

cache.has_key('v')#判断key为v是否存在

cache.get('v')   #获取key为v的缓存



**Django中使用缓存的几种方式**

1. 视图函数中使用缓存

下面这段代码将my_view这个视图函数缓存60*15秒，即15分钟，这个视图中所有的url都会创建一个缓存。



```
from django.views.decorators.cache import cache_page



@cache_page(60 * 15) 

def my_view(request):

  return render(request, 'index.html')
```





但是，需要说明的是，给视图添加缓存是有风险的，如果视图所展示的网页中有经常动态变动的信息，那么被添加缓存命令不可取。



缓存整个视图最实用的场景应该是这个视图所展示的网页的内容基本上不怎么变动，或者说在很长一段时间内不需要变动，这样使用缓存就非常有效。



\2. URLconf中使用缓存

上面说了函数视图使用缓存，但是我们可能还有一种场景，那就是多个 URL 指向同一个函数视图，但是我只想缓存一部分的 URL，这时候就可以采用在 URLconf 中使用缓存，这样就指定了哪些 URL 需要缓存。



下面分别表示了函数视图和类视图的路由中使用缓存的方式，基本一致：

```
from django.views.decorators.cache import cache _page

urlpatterns = [

  url(r'^foo/([0-9]{1,2})/$',cache_page(60 * 15)(my_view)),

  url(r'^$', cache_page(60 * 30)(IndexView.as_view()), name='index'),

]
```

URLconf 使用缓存和视图函数使用缓存需要注意的地方是一样的，因为它们都是缓存整个页面，所有都需要考虑是否整个页面都应该缓存。



\3. 函数中使用缓存

函数中使用缓存是最基本的使用方法，跟在其他非 django 中使用的方式一致，无非就是使用 set() 和 get() 方法。



例如我有一个使用场景：我的博客的文章是使用的 markdown 的格式输入的，所以每次展现到前端之前后端都需要把文章的内容进行一次 markdown 转化，这个渲染的过程难免会有点影响性能，所以我可以使用缓存来存放已经被渲染过的文章内容。具体的代码片段如下：



```
ud = obj.update_date.strftime("%Y%m%d%H%M%S")

md_key = '{}_md_{}'.format(obj.id, ud)

cache_md = cache.get(md_key)

if cache_md:

  md = cache_md

else:

  md = markdown.Markdown(extensions=[

​    'markdown.extensions.extra',

​    'markdown.extensions.codehilite',

​    TocExtension(slugify=slugify),

  ])

  cache.set(md_key, md, 60 * 60 * 12)
```

上面的代码中，我选择文章的 ID 和文章更新的日期作为缓存的 key，这样可以保证当文章更改的时候能够丢弃旧的缓存进而使用新的缓存，而当文章没有更新的时候，缓存可以一直被调用，直到缓存按照设置的过期时间过期。



4.模板中使用缓存

模板中使用缓存是我比较推荐的一种缓存方式，因为使用这种方式可以充分的考虑缓存的颗粒度，细分颗粒度，可以保证只缓存那些适合使用缓存的 HTML 片段。



具体的使用方式如下，首先加载 cache 过滤器，然后使用模板标签语法把需要缓存的片段包围起来即可。



```
{% load cache %}

{% cache 500 ‘cache_name’ %}

    <div></div>

{% endcache %}
```



**总结**

先说一下我在使用缓存的时候遇到的问题，我之前给我的很多视图函数还有URL路由添加了缓存，也就是缓存整个页面，后来发现出问题了，因为我的每个页面都有导航栏，而导航栏上面有登录和登出按钮，这样如果缓存起来的话，就无法让用户显示登录和登出了，并且，有表单的页面也无法提交表单，总之，缓存整个页面是一件有风险的行为。



那么到底哪些时候应该用缓存呢？据我目前的理解，下面这些时候可以用缓存：



- 纯静态页面
- 读取了数据库信息，但是不经常变动的页面，比如文章热门排行榜，这个调用数据库信息并且还要排序的完全可以使用缓存，因为不需要实时展现最新的
- HTML 的片段，比如整个页面都经常变动，但是有个侧边栏不经常变动，就可以缓存侧边栏
- 需要使用复杂逻辑生成的 HTML 片段，使用缓存可以减少多次重复操作



原文地址：

- https://mp.weixin.qq.com/s?__biz=MjM5OTMyODA4Nw==&mid=2247484343&idx=1&sn=9c948fbf3389f5e0f0761164eadcf463&chksm=a73c638f904bea99909afdf53792f0eb66a8bb374b64c16a8ab9297157ac71885d31037d1c63&scene=21#wechat_redirect