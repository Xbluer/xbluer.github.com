---
layout: post
category : note
title: "Python string"
tagline: ""
tags : [Python]
published: true

---
{% include JB/setup %}


Python中没有字符类型，取而代之的是只有一个字符的字符串。

Python的字符串属于不可变类别，即不能原地修改。改变字符串都是通过一定的方式新建字符串。

Python中的字符串不是以某一个特殊字符结尾，而是直接在内存中保存文本和长度。

#### 建立字符串

可以使用单引号和双引号建立字符串常量，二者没有区别。只是若用单引号创建字符串，则字符串内不能有直接使用单引号，需要使用转义字符的方式添加。字符串内也可以像C语言中一样使用转义字符。

在Raw字符串中转义字符失效，这个在正则匹配中比较有效。(__不能以单个反斜杠结尾__)

```
>>> str = r'abc\ndef
>>> print str
abc\ndef
```

另外两种特殊的字符串时Byte字符串和Unicode字符串，分别将Raw字符串前的字母r改为b和u即可。

可以使用三重引号编写多行字符串块，并在代码折行处嵌入换行字符(\n)

#### 分片和索引

略

#### 常用函数

```
len(str)                                # 返回字符串长度
str.find('substr')                      # 返回子字符串的偏移，未找到-1
str.restrip()                           # 清楚行尾的空白
str.replace('old', 'new', [, count])    # 替换
str.split(fs)                           # 分割字符串生成子字符串的列表，以分隔符字符串为准，默认为空格
str.isdigit()                           # 判断字符串是否都是字母
str.lower()                             # 将字符串中所有字符转换为小写
str.endswith('ends')                    # 判断字符串是否以子字符串结尾 
str.join(strlist)                       # 将字符串列表合并成字符串
# ...
```

#### 字符串格式化

有两种形式：

- 字符串格式化表达式：基于C语言中的printf模型。
- 字符串格式化方法调用：与前者功能上有很大重叠，但是相对简明。

##### 字符串格式化表达式：

一、同printf

```
>>> str = 'spam'
>>> print "'%s' has %d characters." % (str, len(str))
'spam' has 4 characters.
>>> '%f, %.2f, %.*f' % (1/3.0, 1/3.0, 4, 1/3.0)
'0.333333, 0.33, 0.3333'
```

`%*`，是占位符，与printf中的基本一致。

可以在运算过程中计算长度和精度。如第二个例子中的`%.*f`

二、基于字典的字符串格式化

```
>>> str = 'spam'
>>> print "'%(x)s' has %(n)d characters." % {'x': str, "n": len(str)}
'spam' has 4 characters.
```

格式化字符串里(x)和(n)引用了右边字典中的建。

##### 字符串格式化调用方法

调用字符串对象的format()，使用主体字符串作为模板，并且接受任意多个表示将要根据模板替换的值的参数。在字符串中，花括号`{}`通过位置({1})或关键字({key})指出替换目标及将要插入的参数。

```
>>> '{motto}, {0} and {food}'.format(42, motto = 3.24, food = [1, 2])
'3.14, 42 and [1, 2]'
```

添加键、属性和偏移量

```
>>> import sys
>>> 'My {1[spam]} runs {0.platform}'.format(sys, {'spam': 'laptop'})
'My laptop runs win32'
>>> 'My {config[spam]} runs {sys.platform}'.format(sys = sys, config = {'spam': 'laptop'})
'My laptop runs win32'
```

格式化字符串中的方括号可以指定列表(或其他的序列)偏移量以执行索引，但是只能有单一的正的偏移才能在格式化字符串的语法中有效，因此不是很通用。


添加具体格式化

与表达式类似的是可以在字符串中添加额外的语法来实现更具体的层级。

```
{fieldname!conversion:formatspec}
```

- fieldname指定参数的一个数字或关键字，可以跟着选择".name"或"[index]"成分引用。
- conversion可以时r、s或者a，分别对应在该值上对应repr、str或ansii内置函数的一次调用。
- formatspec指定如何表示该值，暴多字段宽度、对齐方式、补零、小数点等，并且可以选择数据类型编码结束。
 
```
[\[fill]aalign\][sign][#][0][width][.precision][typecode]
```

align:可以时<、>、=或^，分别表示左对齐、右对齐、一个标记字符后的补充或居中对齐。

formatspec也可以包含嵌套的、只带{}的格式化字符串，从参数列表中动态获取值。(和格式化表达式中的*很相似)
