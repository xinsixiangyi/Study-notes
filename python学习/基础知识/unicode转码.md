# python 的unicode类型的字符串最终转dict





```python
s={"templete": {"industry": "cmes", "type": "DEV", "uuid": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx", "name": "windowsç³»å"}, "params": [{"qutote": "SQL_server_user", "display": "sa", "type_field": "text"}, {"qutote": "SQL_server_password", "display": "topsec5*", "type_field": "text"}]}
a=tool_list.filter(id=3)[0].templatedata
b1 = str(a).decode('utf-8').replace('\'', '\"').decode('unicode-escape').replace('u\"', '\"')
print b1
print type(b1)
c1 = json.loads(b1)
print c1
print type(c1)
x1=c1["templete"]
print x1["name"].encode('raw_unicode_escape').decode()
# d1 = json.dumps(c1)
# print d1
# print type(d1)
```



网络示例：

```
a="[{u'category': u'flow', u'name': u'\u91d1\u5c91\u4e81', u'value': 10.14, u'createtime': u'2019-05-21 00:00:00', u'type': u'40', u'id': 890220}]"
```

形如 a的字符串最终转为list类型，转类型的时候各种报错，而且api接口各种数据不支持报错  "无效数据。期待为字典类型，得到的是 unicode 。"最终进行了如下转换才被接受

```
a="[{u'category': u'flow', u'name': u'\u91d1\u5c91\u4e81', u'value': 10.14, u'createtime': u'2019-05-21 00:00:00', u'type': u'40', u'id': 890220}]"
b1=str(a).decode('utf-8').replace('\'','\"').decode('unicode-escape').replace('u\"','\"')
print b1
print type(b1)
c1= json.loads(b1)
print c1
print type(c1)
d1=json.dumps(c1)
print d1
print type(d1)
```

打印输出：

[{"category": "flow", "name": "金岑亁", "value": 10.14, "createtime": "2019-05-21 00:00:00", "type": "40", "id": 890220}]
<type 'unicode'>
[{u'category': u'flow', u'name': u'\u91d1\u5c91\u4e81', u'value': 10.14, u'createtime': u'2019-05-21 00:00:00', u'type': u'40', u'id': 890220}]
<type 'list'>
[{"category": "flow", "name": "\u91d1\u5c91\u4e81", "value": 10.14, "createtime": "2019-05-21 00:00:00", "type": "40", "id": 890220}]
<type 'str'>