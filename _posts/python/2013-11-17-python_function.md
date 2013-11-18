---
layout: post
category : note
title: "Python function 学习笔记(1)"
tagline: "未完待续"
tags : [Python]
---
{% include JB/setup %}

### 参数
Python的传递参数分为两种：

1. __位置参数__，和其他语言类似，实参与形参位置一一对应，如果有默认实参则放在最后
2. __关键字参数__，实参直接以赋值的方式传递给形参，位置不用一一对应。

_虽然两种方式可以混合使用，但是尽量避免_

参数收集，可以利用元组和字典收集不定数量的参数，关键词：`*`，`**`  
在函数形参前添加`*`可以收集多余的位置参数，`**`则可以收集关键字参数

{% highlight py %}
def print_params(x, y, z=3, *pospar, **keypar):
    print x, y, z
    print pospar
    print keypar

print_params(1, 2, 3, 4, 5, 6, 7, foo = 3, bar = 4)
1, 2, 3
(4, 5, 6, 7)
{'foo' = 3, 'bar' = 4}
{% endhighlight py %}

在语句中也可以使用`*`和`**`，但是作用反转。

### 作用域
list类型的参数在函数内部被修改会影响到调用函数的作用域。其他类型的不会有这个问题。

函数内部可以访问全局变量。如果局部变量与全局变量同名，则全局变量被屏蔽，必须通过`global var`来声明使用，或者`globals()['var']`直接访问  
`globals()`返回全局作用域变量字典，`locals()`返回局部变量字典。

Pyhton中函数是可以嵌套的。闭包。__待续__

