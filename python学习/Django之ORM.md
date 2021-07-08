## Django ORM模型一对一、一对多、多对多的操作详解

### 创建模型

根据图解创建测试模型，models.py文件如下：

```PYTHON
from django.db import models
 
#学生类
class  Student(models.Model):
    id = models.AutoField(primary_key=True)
    sname = models.CharField(max_length=20)
 
    # 一对多外键设置，'多'的模型类设置外键，注意需要带参数on_delete
    cid = models.ForeignKey('Class',on_delete=models.CASCADE)
 
    # 一对一外键设置，哪个模型设置外键都可以，注意需要带参数on_delete
    detail = models.OneToOneField('StudentDetail',on_delete=models.CASCADE)
 
#学生信息类
class StudentDetail(models.Model):
    id = models.AutoField(primary_key=True)
    height = models.IntegerField()
    email = models.EmailField()
    memo = models.TextField(max_length=100)
 
#班级类
class Class(models.Model):
    id = models.AutoField(primary_key=True)
    cname = models.CharField(max_length=20)
    cdata = models.DateField()
 
#老师类
class Teacher(models.Model):
    id = models.AutoField(primary_key=True)
    tname = models.CharField(max_length=20)
 
    #多对多外键设置，哪个模型类创建外键都可以，注意没有on_delete参数
    cid = models.ManyToManyField(Class)
```

\#注意：设置外键的方法中，第一个参数为外键对应的类名，一般加上引号。如果不加引号，那该类必须创建在主表前面

### 一对一、一对多、多对多的正向反向查询

```python
from django.shortcuts import render,HttpResponse
from django.db.models import F,Q    #测试学习F，Q查询
 
from app1.models import Student,StudentDetail,Class,Teacher
# Create your views here.
def index(request):
 
    #一对一  Student ---> StudentDetail
    #正向
    print(Student.objects.filter(id__gt=2)[0].detail)       #调用学生对象的detail属性，该属性表示外键映射副表中对应的对象
    print(StudentDetail.objects.filter(student__id__gt=1))      #过滤学生信息表查询数据，过滤参数为 "主表名__条件"
 
    #反向
    print(StudentDetail.objects.get(id=1).student)      #这个同上--但是副表要调用对应主表的信息对象，需要使用  .主表名(全小写)
    print(Student.objects.filter(detail__id
                                 
                                 
                                 
                                 
                                 __exact=1))  #同上  -- 主表调用副表直接使用添加外键的属性detail就行，这个
# 属性就是副表中对应的一条数据对象
 
    #一对多  Student ---> Class
    #正向：
    print(Student.objects.get(id=1).cid)        #使用主表添加的外键属性，获取副表中的对应数据对象
    print(Class.objects.filter(student__id=1))  #副表过滤主表条件的参数，使用student可以调用主表对象，student__id
# 是调用的student对象的id进行筛选
 
    # 反向
    print(Class.objects.get(id=1).student_set.all())    #副表调用主表信息，使用   主表名(全小写)__set   ,这个其实是
# 一个副表对象在主表中的映射管理器，类似Student.objtects中的objects，这儿objects是控制管理器
    print(Student.objects.filter(cid__id=1))        #这个同上
 
    #多对多    Teacher --> Class
    # 正向：
    print(Teacher.objects.get(id=1).cid.all())      #多对多其实可以对应一对多，两者大体一致，只不过主表的外键属性和副表
# 的teacher__set都是相应的映射管理器，它内部其实都是对应的中间表的进行的关联映射
    print(Class.objects.filter(teacher__id=1))
 
    #反向
    print(Class.objects.get(id=1).teacher_set.all())
    print(Teacher.objects.filter(Q(cid__id=1) | ~Q(cid__id=2)))     #这儿使用到了Q查询，当使用或的条件查询语句时使用，并
# 且F,Q查询与关键字参数一起使用时，F,Q查询必须写到关键字参数前面
 
 
    #补充============
    #多对多关系模型添加移除中间表关系 Teacher -- >Class
    #本质是给中间表添加，移除数据
    #正向：
    # 添加
    Teacher.objects.get(id=5).cid.add(*Class.objects.filter(id__gt = 3))
    #移除
    Teacher.objects.get(id=5).cid.remove(*Class.objects.filter(id__gt=1))
    #清空对象关系
    Teacher.objects.get(id=5).cid.clear()
    #重新设置关系     #添加新关系，删除多余关系
    Teacher.objects.get(id=5).cid.set(list(Class.objects.filter(id__gt=5)) )#参数为一个可迭代对象就可以
 
 
    return HttpResponse('success')
```

注意：在上面文件中，我提到的映射管理器，其实形似objects这个控制管理器，同样可以使用all(),values(),value_list()方法，对结果进行查询。当然这个映射管理器也可以添加映射，在多对多中通过映射管理器的add方法添加一个新的关系映射，在一对多中通过映射管理器的create方法，可以添加新的副表数据。以下为测试实例：



```python3
    Class.objects.get(id=3).student_set.create(sname='aaaaa',detail_id=7) 
#在id=3的班级添加一个学生，该学生会被自动创建，并且添加了班级和学生之间的映射关系
 
    Class.objects.get(id=3).teacher_set.add(Teacher.objects.get(id=4)) 
#在多对多中通过add添加外键映射关系，并且在中间表中生成对应关系数据
```