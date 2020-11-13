# Typora设置字体颜色大小等属性

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