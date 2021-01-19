# Django 多数据库联用

http://www.ziqiangxuetang.com/django/django-registration.html)

本文讲述在一个 django project 中使用多个数据库的方法, 多个数据库的联用 以及多数据库时数据导入导出的方法。

直接给出一种简单的方法吧，想了解更多的到官方教程，[点击此处](https://docs.djangoproject.com/en/dev/topics/db/multi-db/)

代码文件下载：![img](http://www.ziqiangxuetang.com/static/ueditor/dialogs/attachment/fileTypeImages/icon_rar.gif)[project_name.zip](http://zqxt.oss-cn-beijing.aliyuncs.com/media/uploads/files/project_name_20170501173221_818.zip)（2017年05月01日更新）

## 1. 每个app都可以单独设置一个数据库

settings.py中有数据库的相关设置，有一个默认的数据库 default,我们可以再加一些其它的，比如：

```python
# Database
# https://docs.djangoproject.com/en/1.8/ref/settings/#databases
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    },
    'db1': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'dbname1',
        'USER': 'your_db_user_name',
        'PASSWORD': 'yourpassword',
        "HOST": "localhost",
    },
    'db2': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'dbname2',
        'USER': 'your_db_user_name',
        'PASSWORD': 'yourpassword',
        "HOST": "localhost",
    },
}
 
# use multi-database in django
# add by WeizhongTu
DATABASE_ROUTERS = ['project_name.database_router.DatabaseAppsRouter']
DATABASE_APPS_MAPPING = {
    # example:
    #'app_name':'database_name',
    'app1': 'db1',
    'app2': 'db2',
}
```



在project_name文件夹中存放 database_router.py 文件，内容如下：

```python
# -*- coding: utf-8 -*-
from django.conf import settings
 
DATABASE_MAPPING = settings.DATABASE_APPS_MAPPING
 
 
class DatabaseAppsRouter(object):
    """
    A router to control all database operations on models for different
    databases.
 
    In case an app is not set in settings.DATABASE_APPS_MAPPING, the router
    will fallback to the `default` database.
 
    Settings example:
 
    DATABASE_APPS_MAPPING = {'app1': 'db1', 'app2': 'db2'}
    """
 
    def db_for_read(self, model, **hints):
        """"Point all read operations to the specific database."""
        if model._meta.app_label in DATABASE_MAPPING:
            return DATABASE_MAPPING[model._meta.app_label]
        return None
 
    def db_for_write(self, model, **hints):
        """Point all write operations to the specific database."""
        if model._meta.app_label in DATABASE_MAPPING:
            return DATABASE_MAPPING[model._meta.app_label]
        return None
 
    def allow_relation(self, obj1, obj2, **hints):
        """Allow any relation between apps that use the same database."""
        db_obj1 = DATABASE_MAPPING.get(obj1._meta.app_label)
        db_obj2 = DATABASE_MAPPING.get(obj2._meta.app_label)
        if db_obj1 and db_obj2:
            if db_obj1 == db_obj2:
                return True
            else:
                return False
        return None
 
    # for Django 1.4 - Django 1.6
    def allow_syncdb(self, db, model):
        """Make sure that apps only appear in the related database."""
 
        if db in DATABASE_MAPPING.values():
            return DATABASE_MAPPING.get(model._meta.app_label) == db
        elif model._meta.app_label in DATABASE_MAPPING:
            return False
        return None
 
    # Django 1.7 - Django 1.11
    def allow_migrate(self, db, app_label, model_name=None, **hints):
        print db, app_label, model_name, hints
        if db in DATABASE_MAPPING.values():
            return DATABASE_MAPPING.get(app_label) == db
        elif app_label in DATABASE_MAPPING:
            return False
        return None
```



这样就实现了指定的 app 使用指定的数据库了,当然你也可以多个sqlite3一起使用，相当于可以给每个app都可以单独设置一个数据库！如果不设置或者没有设置的app就会自动使用默认的数据库。

## 2.使用指定的数据库来执行操作

在查询的语句后面用 using(dbname) 来指定要操作的数据库即可

```python
# 查询
YourModel.objects.using('db1').all() 
或者 YourModel.objects.using('db2').all()
 
# 保存 或 删除
user_obj.save(using='new_users')
user_obj.delete(using='legacy_users')
```

## 3.多个数据库联用时数据导入导出

使用的时候和一个数据库的区别是:

**如果不是defalut(默认数据库）要在命令后边加 --database=数据库对应的settings.py中的名称** 如： --database=db1  或 --database=db2

**数据库同步（创建表）**

```shell
# Django 1.6及以下版本
python manage.py syncdb #同步默认的数据库，和原来的没有区别
 
# 同步数据库 db1 (注意：不是数据库名是db1,是settings.py中的那个db1，不过你可以使这两个名称相同，容易使用)
python manage.py syncdb --database=db1
 
# Django 1.7 及以上版本
python manage.py migrate --database=db1
```

**数据导出**

```shell
python manage.py dumpdata app1 --database=db1 > app1_fixture.json
python manage.py dumpdata app2 --database=db2 > app2_fixture.json
python manage.py dumpdata auth > auth_fixture.json
```

**数据库****导入**

```shell
python manage.py loaddata app1_fixture.json --database=db1
python manage.py loaddata app2_fixture.json --database=db2
```

