# 基于Django模板中的数字自增(详解)



**Django框架的模板提供了{% for %} 标签来进行循环**

例如对集合进行循环是比较简单的

```
{% for row in v1 %}``<``div``>{{row.name}}</``div``>``{% endfor %}
```

但是在Django中,并不直接支持形如"int i = 0;i<100;i++" 这样的循环,Django有自己的自增方法

假设v1内有2个元素

1,从1开始正向自增 结果1,2

```
{% for row in v1 %}``<``div``>{{forloop.counter}}</``div``>``{% endfor %}
```

2，从0开始正向自增 结果0,1

```
{% for row in v1 %}``<``div``>{{forloop.counter0}}</``div``>``{% endfor %}
```

3，自减到1 结果2,1

```
{% for row in v1 %}``<``div``>{{forloop.revcounter}}</``div``>``{% endfor %}
```

4,自减到0 结果1,0

```
{% for row in v1 %}``<``div``>{{forloop.revcounter0}}</``div``>``{% endfor %}
```

5，是否是最后一个 结果False，True

```
{% for row in v1 %}``<``div``>{{forloop.last}}</``div``>``{% endfor %}
```

6，是否是第一个 结果True，False

```
{% for row in v1 %}``<``div``>{{forloop.first}}</``div``>``{% endfor %}
```

7，如果有多层循环，返回上层循环的全部取值的结果

```
{% for i in v1 %}``{% for row in v1 %}``<``div``>{{forloop.parentloop}}</``div``>``{% endfor %} ``{% endfor %}
```

循环结果

![img](https://img.jbzj.com/file_images/article/201709/2017090509175111.png)

