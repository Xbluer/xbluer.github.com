---
layout: post
category : note
title: "DrRacket 载入模块"
tagline: "sicp学习笔记"
tags : [scheme, lisp, sicp]
published: true
---
{% include JB/setup %}


学习《sicp》的时候遇到一个问题：需要使用的函数定义的其他文件中。原先处理的方式只是简单的copy，不能像Python那样直接`import`，不是很方便。现在google以一下发现有两种方式解决这个问题。

-------------------------

### 方法一：require 

文件"foo_lib.rkt"

```
#lang racket

(define x 3)
(define y 4)
(define z 5)
(provide (x y))
(display "You include foo_lib.")
```

文件"foo.rkt"

```
#lang racket
(require "foo_lib.rkt")

x
y
;z ; error
(+ x y)
```

执行结果：

```
You include foo_lib.
3
4
7
```

__说明__：

- 被包含的文件"foo_lib.rkt"中要用关键字`provide`，声明对`require`可见的变量。如果"foo_lib.rkt"中定义了变量`z`，但是没有传给`provide`，则"foo.rkt"文件不可以使用变量`z`。
- 被导入的文件会被完全执行。所以执行结果会打印出`You include foo_lib.`。这一点和Python相同。
- "foo.rkt"使用关键字`require`导入需要导入的模块。模块名称中可以添加路径名，如：`(require "../xx_lib.rkt")`。

----------------------------

### 方法二：include

文件"bar_lib.rkt"

```
; no #lang racket
(define hello "Hello World")
(display "You include bar_lib."
```

文件"bar.rkt"

```
#lang racket
(require racket/include)
(include "bar_lib.rkt")
(display hello)
```

执行结果：

```
You include bar_lib.
Hello World
```

__说明：__

- "bar_lib.rkt"中没有`#lang racket`声明。文件中定义的所有变量对"bar"都是可见的，不需要`provide`关键字。
- 文件"bar.rkt"需要两步导入模块"bar_lib.rkt"。
首先，导入include这个内置函数`(require racket/include)`；然后导入目标模块`(include "bar_lib.rkt")`。
- 同先前的上一种方法，被导入的文件会被完全执行。

------------------------------------------------
###两种方法选择:

一般来说两者可以达到相同的目的。如果只是临时性的使用其他文件中定义的变量则使用require即可。但是对于一些非常通用的定义，例如：fib,gcd等，可能在许多地方用到，则最好将这些定义放置在一个专用的目录或文件中。

---------------------
__参考：__

1. <http://stackoverflow.com/questions/4809433/including-an-external-file-in-racket>
2. <http://docs.racket-lang.org/reference/require.html>
