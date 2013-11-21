---
layout: post
category : note
title: "Python 模块"
tagline: ""
tags : [Python]
published: false
---
{% include JB/setup %}

模块只会被导入一次。如果需要强制性导入多次，Py2.X可以用`reload`函数，Py3.X可以使用`exec`函数。模块在第一次导入时被执行。

`\_\_name\_\_`变量可以检查出模块本身时作为程序运行还是导入到其他程序中。如果`\_\_name\_\_==\_\_main\_\_`，模块作为程序运行的。


包：另一类模块，可以简单的看成是一个包含了`\_\_init\_\_.py`文件的目录。一个包可以包含多个模块。

`dir`函数，将对象(以及模块的所有函数、类、变量等)所有特性全部列出来。

`\_\_all\_\_`变量定义了模块的公有接口，即：`from xxx import *`可以导入的所有函数或变量名。如果没有定义`\_\_all\_\_`则用`import *`默认将所有不以下滑线开头的全局名称。
