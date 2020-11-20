## 合并列表中key相同的字典

### 1.第一种：

#### 	问题：

现有list：

list1 = [{a: 123}, {a: 456},{b: 789}]

合并成：

list2 = [{a: [123,456]},{b: [789]}]

#### 	解决方案：

```python
from collections import defaultdict
lst = [{'a': 123}, {'a': 456},{'b': 789}]

dic = {}
for _ in lst:
    for k, v in _.items():
        dic.setdefault(k, []).append(v)

print [{k:v} for k, v in dic.items()]
```

### 2.第二种

#### 问题:多字典嵌套

```json
[
    {
        "name": "前端知识",
        "sub_categorys": [
            {
                "name": "Vue",
                "sub_categorys": []
            },
            {
                "name": "通用知识",
                "sub_categorys": []
            }
        ]
    },
    {
        "name": "数据库",
        "sub_categorys": [
            {
                "name": "Nosql",
                "sub_categorys": [
                    {
                        "name": "mongoDB",
                        "sub_categorys": []
                    },
                    {
                        "name": "redis",
                        "sub_categorys": []
                    }
                ]
            },
            {
                "name": "关系型",
                "sub_categorys": [
                    {
                        "name": "mysql",
                        "sub_categorys": []
                    }
                ]
            }
        ]
    }
]
```

解决方案

```python
from collections import defaultdict
def merge(old_data):
	new_dict = {}
	for i in old_data:
		# print(i.get('sub_categorys'))
		if i.get('sub_categorys')==None:
			new_dict.setdefault(i['name'], [])
		else:
			# print(i.get('sub_categorys'))
			if i.get('sub_categorys'):
				new_dict.setdefault(i['name'], []).append((i.get('sub_categorys'))[0])
			else:
				new_dict.setdefault(i['name'], [])
	# print(new_dict)
	new_list = []
	for x, y in new_dict.items():
		new_list.append({'name': x, 'sub_categorys': y})

	return new_list
```

