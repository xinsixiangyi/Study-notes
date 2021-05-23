# Typora设置

### Typora设置字体颜色大小等属性

由于Typora或者说markdown中没有直接的语法支持文字的颜色，大小，字体等属性的设置，就需要“曲线救国”，因为Typora支持内嵌html的语法，那么只要插入一条html代码即可（直接在Typora的markdown文件中使用即可）：

```html
<span style='color:文字颜色;background:背景颜色;font-size:文字大小;font-family:字体;'>文字</span>
```

使用时将“文字”替换为需要修改属性的文本即可
由于此处不直接支持html的语法，故截图说明效果：
![Typora设置文字属性](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL00wMTBLL1R5cG9yYUBtYXN0ZXIvdVBpYy8yMDIwMDQyMy1UeXBvcmElRTglQUUlQkUlRTclQkQlQUUlRTYlOTYlODclRTUlQUQlOTclRTUlQjElOUUlRTYlODAlQTcucG5n?x-oss-process=image/format,png)
具体使用如下：

![TyporaDemo](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL00wMTBLL1R5cG9yYUBtYXN0ZXIvdVBpYy8yMDIwMDQyNy1UeXBvcmFEZW1vLmdpZg)

需要更快的体验可以参考一下：
[使用Snippets加速修改Typora中文字属性](https://blog.csdn.net/qq_43444349/article/details/105716470)
另外若需要设置标题或者文本居中，可以参考这两篇

- [Typora设置标题居中](https://blog.csdn.net/qq_43444349/article/details/106366671)
- [Typora设置文本居中](https://blog.csdn.net/qq_43444349/article/details/106366895)

### Typora导入导出扩展

#### 安装pandoc扩展程序

进入[pandoc官网](https://github.com/jgm/pandoc/releases/tag/2.2.1)，点击下载适合自己操作系统的版本

![image-20210510162817143](C:\Users\罗永强\AppData\Roaming\Typora\typora-user-images\image-20210510162817143.png)

![这里写图片描述](https://www.pianshen.com/images/125/f481bdc275134c3bfb2a935fe6e8373d.JPEG)

选取第一个选项，第二个选项将需要获取管理员权限，点击安装。

安装为默认选项，没有可修改项，安装默认到C盘