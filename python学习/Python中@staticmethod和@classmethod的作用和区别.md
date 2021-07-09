## Python中@staticmethod和@classmethod的作用和区别



ppython有3种方法，静态方法（staticmethod），类方法（classmethod）和实例方法。下面用代码举例。

### 基础：

对于一般的函数foo(x)，它跟类和类的实例没有任何关系，直接调用foo(x)即可。

```python
# -*- coding:utf-8 -*-
def foo(x):
    print("running foo(%s)" % x)

foo("test")
```

在类A里面的实例方法foo(self, x)，**第一个参数是self**，我们需要有一个A的实例，才可以调用这个函数。

```python
# -*- coding:utf-8 -*-
class A:
    def foo(self, x):
        print("running foo(%s, %s)" % (self, x))

# A.foo(x) 这样会报错
a = A()
a.foo("test")
```

当我们需要和类直接进行交互，而不需要和实例进行交互时，类方法是最好的选择。类方法与实例方法类似，但是传递的不是类的实例，而是类本身，**第一个参数是cls**。**我们可以用类的实例调用类方法，也可以直接用类名来调用。**

```python
# -*- coding:utf-8 -*-
class A:
    class_attr = "attr"
    
    def __init__(self):
        pass
        
    @classmethod
    def class_foo(cls):
        print("running class_foo(%s)" % (cls.class_attr))

a = A()
a.class_foo()
A.class_foo()
```

静态方法类似普通方法，参数里面不用self。这些方法和类相关，但是又不需要类和实例中的任何信息、属性等等。如果把这些方法写到类外面，这样就把和类相关的代码分散到类外，使得之后对于代码的理解和维护都是巨大的障碍。而静态方法就是用来解决这一类问题的。

比如我们检查是否开启了日志功能，这个和类相关，但是跟类的属性和实例都没有关系。

```python
# -*- coding:utf-8 -*-
log_enabled = True

class A:
    class_attr = "attr"
    
    def __init__(self):
        pass
        
    @staticmethod
    def static_foo():
        if log_enabled:
            print("log is enabled")
        else:
            print("log is disabled")
        

A.static_foo()
```

### 进阶:

类中最常用的方法是实例方法, 即通过通过实例作为第一个参数的方法。
举个例子，一个基本的实例方法就向下面这个:

```python
class Kls(object):
    def __init__(self, data):
        self.data = data
    def printd(self):
        print(self.data)
ik1 = Kls('arun')
ik2 = Kls('seema')
ik1.printd()
ik2.printd()
```

这会给出如下的输出:

```python
arun
seema
```

**如果现在我们想写一些仅仅与类交互而不是和实例交互的方法**会怎么样呢? 我们可以在类外面写一个简单的方法来做这些，但是这样做就扩散了类代码的关系到类定义的外面. 如果像下面这样写就会导致以后代码维护的困难:

```python
def get_no_of_instances(cls_obj):
    return cls_obj.no_inst
class Kls(object):
    no_inst = 0
    def __init__(self):
        Kls.no_inst = Kls.no_inst + 1
ik1 = Kls()
ik2 = Kls()
print(get_no_of_instances(Kls))
```

输出:

```
2
```

@classmethod
我们要写一个只在类中运行而不在实例中运行的方法. 如果我们想让方法不在实例中运行，可以这么做:

```python
def iget_no_of_instance(ins_obj):
    return ins_obj.__class__.no_inst
class Kls(object):
    no_inst = 0
    def __init__(self):
    Kls.no_inst = Kls.no_inst + 1
ik1 = Kls()
ik2 = Kls()		
print iget_no_of_instance(ik1)
   123456789123456789
```

输出

```
2
```

在Python2.2以后可以使用@classmethod装饰器来创建类方法.

```python
class Kls(object):
    no_inst = 0
    def __init__(self):
        Kls.no_inst = Kls.no_inst + 1
    @classmethod
    def get_no_of_instance(cls_obj):
        return cls_obj.no_inst
ik1 = Kls()
ik2 = Kls()
print ik1.get_no_of_instance()
print Kls.get_no_of_instance()
   12345678910111234567891011
```

输出:

```
2
2
```

这样的好处是: 不管这个方式是从实例调用还是从类调用，它都用第一个参数把类传递过来.
@staticmethod
经常有一些跟类有关系的功能但在运行时又不需要实例和类参与的情况下需要用到静态方法. 比如更改环境变量或者修改其他类的属性等能用到静态方法. 这种情况可以直接用函数解决, 但这样同样会扩散类内部的代码，造成维护困难.

比如这样:

```python
IND = 'ON'
def checkind():
    return (IND == 'ON')
class Kls(object):
     def __init__(self,data):
        self.data = data
def do_reset(self):
    if checkind():
        print('Reset done for:', self.data)
def set_db(self):
    if checkind():
        self.db = 'new db connection'
        print('DB connection made for:',self.data)
ik1 = Kls(12)
ik1.do_reset()
ik1.set_db()
```

输出:

```
Reset done for: 12
DB connection made for: 12
```

如果使用@staticmethod就能把相关的代码放到对应的位置了.

```python
IND = 'ON'
class Kls(object):
    def __init__(self, data):
        self.data = data
    @staticmethod
    def checkind():
        return (IND == 'ON')
    def do_reset(self):
        if self.checkind():
            print('Reset done for:', self.data)
    def set_db(self):
        if self.checkind():
            self.db = 'New db connection'
        print('DB connection made for: ', self.data)
ik1 = Kls(12)
ik1.do_reset()
ik1.set_db()
```

输出:

```
Reset done for: 12
DB connection made for: 12
```



下面这个更全面的代码和图示来展示这两种方法的不同
@staticmethod 和 @classmethod的不同

```python
class Kls(object):
    def __init__(self, data):
        self.data = data
    def printd(self):
        print(self.data)
    @staticmethod
    def smethod(*arg):
        print('Static:', arg)
    @classmethod
    def cmethod(*arg):
        print('Class:', arg)

>>> ik = Kls(23)
>>> ik.printd()
23
>>> ik.smethod()
Static: ()
>>> ik.cmethod()
Class: (<class '__main__.Kls'>,)
>>> Kls.printd()
TypeError: unbound method printd() must be called with Kls instance as first argument (got nothing instead)
>>> Kls.smethod()
Static: ()
>>> Kls.cmethod()
Class: (<class '__main__.Kls'>,)
```

![这里写图片描述](https://img-blog.csdn.net/20160922172202855)

