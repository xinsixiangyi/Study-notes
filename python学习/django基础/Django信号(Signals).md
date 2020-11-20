## Django信号(Signals)

Django 提供一个了“信号分发器”机制，允许解耦的应用在框架的其它地方发生操作时会被通知到。 通俗而讲Django信号的工作原理就是当某个事件发生的时候会发出一个信号(signals), 而监听这个信号的函数(receivers)就会立即执行。Django信号的应用场景很多，尤其是用于不同模型或程序间的联动。常见例子包括创建User对象实例时创建一对一关系的UserProfile对象实例，或者每当用户下订单时触发给管理员发邮件的动作。

### **Django信号的一个简单例子**

假设我们有一个如下User模型，我们希望每次有User对象新创建时都打印出有新用户注册的提示信息，我们可以使用Django信号(signals）轻松实现。我们的信号发送者sender是User模型，每当User模型执行post_save动作时就会发出信号。此时我们自定义的create_user函数一旦监听到User发出的post_save信号就会执行，先通过if created判断对象是新创建的还是被更新的；如果对象是新创建的，就会打印出提示信息。

*# models.py*

```python
from django.db import models
from django.db.models import signals
from django.dispatch import receiver

class User(models.Model):
    name = models.CharField(max_length=16)
    gender = models.CharField(max_length=32, blank=True)

def create_user(sender, instance, created, **kwargs):
    if created:
        print("New user created!")
post_save.connect(create_user, sender=User)
```

在上例中我们使用了信号(post_save)自带的connect的方法将自定义的函数与信号发出者(sender)User模型进行了连接。在实际应用中一个更常用的方式是使用@receiver[装饰器](https://link.zhihu.com/?target=http%3A//mp.weixin.qq.com/s%3F__biz%3DMjM5OTMyODA4Nw%3D%3D%26mid%3D2247484195%26idx%3D1%26sn%3D0f92015bf28c53ad4ae48a5cc1d9e0da%26chksm%3Da73c631b904bea0d17e994b76ddbb78b18a3bdada1162dfdec4f023efe3bd8bca4dfda13018f%26scene%3D21%23wechat_redirect)实现发送者与监听函数的连接，如下所示。@receiver(post_save, sender=User)读起来的意思就是监听User模型发出的post_save信号。

```python
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver

class User(models.Model):
    name = models.CharField(max_length=16)
    gender = models.CharField(max_length=32, blank=True)

@receiver(post_save, sender=User)
def create_user(sender, instance, created, **kwargs):
    if created:
        print("New user created!")
```

### **利用Django信号实现不同模型的联动更新**

我们再来看一个复杂一点的例子。我们有一个Profile模型，与User模型是一对一的关系。我们希望创建User对象实例时也创建Profile对象实例，而使用post_save更新User对象时不创建新的Profile对象。这时我们就可以自定义create_user_profile和save_user_profile两个监听函数，同时监听sender(User模型)发出的post_save信号。由于post_save可同时用于模型的创建和更新，我们用if created这个判断来加以区别。

```python
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver

class Profile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    birth_date = models.DateField(null=True, blank=True)

@receiver(post_save, sender=User)
def create_user_profile(sender, instance, created, **kwargs):
   if created:
       Profile.objects.create(user=instance)

@receiver(post_save, sender=User)
def save_user_profile(sender, instance, **kwargs):
    instance.profile.save()
```

### **Django常用内置信号**

之前的例子中我们使用的都是post_save信号，即在模型调用save()方法后才发送信号。Django其它常用内置信号还包括:

- django.db.models.signals.pre_save & post_save在模型调用 save()方法之前或之后发送。
- django.db.models.signals.pre_init& post_init在模型调用_init_方法之前或之后发送。
- django.db.models.signals.pre_delete & post_delete在模型调用delete()方法或查询集调用delete() 方法之前或之后发送。
- django.db.models.signals.m2m_changed在模型多对多关系改变后发送。
- django.core.signals.request_started & request_finished Django建立或关闭HTTP 请求时发送。

### **如何正确放置Django信号的监听函数代码**

在之前案例中，我们将Django信号的监听函数写在了models.py文件里。当一个app的与信号相关的自定义监听函数很多时，此时models.py代码将变得非常臃肿。一个更好的方式把所以自定义的信号监听函数集中放在app对应文件夹下的signals.py文件里，便于后期集中维护。

假如我们有个account的app，包含了User和Pofile模型，我们不仅需要在account文件夹下新建signals.py，还需要修改account文件下apps.py和__init__.py，以导入创建的信号监听函数。



```python
# account/signals.py

from django.db.models.signals import post_save
from django.dispatch import receiver
from .models import User, Profile

@receiver(post_save, sender=User)
def create_user_profile(sender, instance, created, **kwargs):
  if created:
      Profile.objects.create(user=instance)

@receiver(post_save, sender=User)
def save_user_profile(sender, instance, **kwargs):
    instance.profile.save()
    
# account/apps.py

from django.apps import AppConfig

class AccountConfig(AppConfig):
    name = 'account'

    def ready(self):
        import account.signals
        
# account/__init__.py

default_app_config = 'account.apps.AccountConfig'
```

### 实战：

```




```

