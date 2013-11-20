---
layout: post
category : note
title: "Python while和for循环"
tagline: ""
tags : [Python]
published: true
---
{% include JB/setup %}

#### while 循环

```
while <test>:
    <statement1>
else:
    <statement2>
```

对于可选分支`else`只有当循环正常离开才会执行，即`test`语句被检测为`False`而退出循环。即便主循环没有被执行也会执行`else`中语句。如果主循环中有`break`语句被执行而退出循环，则不执行`else`部分。

Python添加循环中`else`语法的目的是：不通过设定和检查标志位或变量来捕捉循环的**循环的另一个出口**。

```
# 打印一个数的因子(非1、自身)。
# c style
def getFactor(num):
    isPrime = True
    x = num // 2
    while x > 1:
        if num % x == 0:
            print x, 'is a factor of', num
            isPrime = False
            break
        x -= 1
    if isPrime:
        print num, 'is a prime.'
    
# python only

def getFactor_(num):
    x = num // 2
    while x > 1:
        if num % x == 0:
            print x, 'is a factor of', num
            break
        x -= 1
    else:
        print num, 'is a prime.'
```

`pass`语句，占位语句，没有任何意义。

#### for 循环

```
for <target> in <object>
    <statement1>
else:
    <statement2>
```

for循环是一个通用的序列迭代器。for循环执行时，会将__序列对象__的元素逐个赋给目标，然后为每个元素执行循环主体。for循环也支持可选的`else`语句，用法和while相同。

```
L = [1, 2, 3, 4, 5]
D = dict(a = 1, b = 2, c = 3)

# list, string 等序列对象
for x in L:
    print (x)

# dict
for key in D:
    print ('key:', key, ' value:', D[key])

# 元组赋值
T = [(1, 2), (3, 4), (5, 6)]
for (a, b) in T:
    print (a, b)
```

__for循环比while循环容易书写，且执行速度更快__

##### range内置函数，返回一系列连续的整数，可以作为for的索引。

```
# 利用索引迭代序列对象。
for ix in range(len(L)):
    print (L[ix], end = '')
print('')
# 跳过一些元素，此处只显示1,3,5
for ix in range(0, len(L), 2):
    print (L[ix], end = '')
print('')
# 利用索引迭代可以修改序列对象。for x in obj这种形式不能修改序列对象本身。
print (L)
for ix in range(1, len(L), 2):
    L[ix] *= 2
print (L)

# result:
12345
135
[1, 2, 3, 4, 5]
[1, 4, 3, 8, 5]
```

##### zip和map可以并行遍历
zip(L1, L2)。从两个列表中提取元素配对。zip可以接受任何序列，包括文件。zip会以最短序列的长度截断所得到的元组。

```
print('')
print('')
L1 = [1, 2, 3, 4]
L2 = [5, 6, 7, 8]

for (a, b) in zip(L1, L2):
    sum = a + b
    print (a, '+', b, '=', sum)
```

Python2.x中可以使用内置map函数，用类似的方式将序列元素配对起来，但是参数长度不同时，则为较短的序列用None不齐。Pthont3.0不再支持。

```
>>> s1 = 'abc'
>>> s2 = 'abcd'
>>> map(None, s1, s2)
[('a', 'a'), ('b', 'b'), ('c', 'c'), (None, 'd')]
```

##### enumerate 产生偏移和元素
enumerate函数产生的时生成器对象。

```
S = 'spam'
for (offset, item) in enumerate(S):
    print (itme, 'appears at offset', offset)

#
s appears at offset 0
p appears at offset 1
a appears at offset 2
m appears at offset 3
```
