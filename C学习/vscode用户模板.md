# 在vs code中添加最简单的模板

之前使用pycharm的时候可以在设置中添加各种模板，这样在新建一个py文件时就会预先添加好模板中的代码或注释，十分方便。最近打算学习C语言，下载并配置好了vs code，就在想vs code中是否有这种功能，果不其然是有的。

通过 文件-首选项-用户代码片段 的顺序进入，然后找到自己需要使用的语言并打开。这里我用的C作示范，其他语言应该都是类似的。

![img](https://pic3.zhimg.com/80/v2-4c6cb26f87843b8d125e5fe5a0f3e246_1440w.jpg)文件-首选项-用户代码片段

![img](https://pic3.zhimg.com/80/v2-0616f3c2b62d69730aac371f2b0974f6_1440w.jpg)

这样就打开了一个json文件，这里有官方自带的一个模板

```text
{   
    "Print to console": {
	 "prefix": "log",
	 "body": [
             "console.log('$1');",
	     "$2"
	],
	 "description": "Log output to console"
     }
}
```

这里我们可以通过对body键的值进行修改来修改模板的内容，其中的内容使用字符串格式，逗号表示换行，最终我写出的模板如下。

```text
{
	"C header file": {        
		"prefix": "#inc",
        "body": [
			    "#include <stdio.h>",
		    	"int main()",
				"{",
				"\treturn 0;",
				"}",
			],
		"description": "C header file define" 
}
```

将这个json文件保存（ctrl+s）后打开一个c的文件，输入prefix键所对应的值就可以找到写好的模板，选中后整个模板就打出来了。

![img](https://pic1.zhimg.com/80/v2-9df0cfe304ce21d7a9d4a073f29cb1d0_1440w.png)我写的模板中prefix对应的值为#inc，就可以通过输入关键字来找到这个模板，后面的说明来自description中的内容

![img](https://pic4.zhimg.com/80/v2-56ee4b2844ecd1c5b3332259f34d9deb_1440w.jpg)