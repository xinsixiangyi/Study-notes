C语言调用函数

就是先定义并声明函数，du之后再根据定义函数的格式调用。

下面举例来说明函数调用方法：

```c
#include<stdio.h>

int fun(int x, int y); // 函数声dao明，如果函数写在被调用处之前，可以不用声明

void main()

{

  int a=1, b=2, c;

  c = fun(a, b); // 函数的调用，调用自定义函数fun，其中a，b为实际参数，传递给被调用函数的输入值

}

// 自定义函数fun

int fun(int x, int y) // 函数首部

{ // {}中的语言为函数体

  return x>y ? x : y; // 返回x和y中较大的一个数

}
```

