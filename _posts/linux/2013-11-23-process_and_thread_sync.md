---
layout: post
category : note
title: "进程与线程 同步——《现代操作系统》"
tagline: ""
tags : [Linux, OS]
published: true
---
{% include JB/setup %}

#### 竞争条件
两个或多个线程读写某些共享数据结构，而最后的结果取决于进程运行的精确时序。

-------------------
#### 临界区
对共享内存进行访问的程序片段。

-------------------
#### 忙等互斥
1. 屏蔽中断：仅对只有一个CPU的系统有效，多个或多核CPU下无效。
2. 锁变量：无效。
3. 严格轮转法：可行，但要求进程有序进入临界区，且临界区外的进程会阻塞其他进程。
4. Peterson方法：可行，在(3)的基础上，对进程进入临界区的次序不做假设，也不会存在临界区外的进程阻塞其他进程。
5. TSL指令：可行，XCHG可实现相同功能。

__只有在有理由认为等待时间是非常短的情形下，才能使用忙等待。__


-------------------
#### 睡眠与唤醒

-------------------
#### 信号量
使用一个整型变量累计唤醒次数。检查数值、修改数值以及可能发生的睡眠唤醒操作都是原子操作。

-------------------
#### 互斥量
信号量的简化版，允许或阻塞对临界区的访问。

-------------------
#### 条件变量
允许线程由一些未到达的条件而阻塞。条件变量不会保存在内存中，若向一个信号量传递一个没有线程等待的条件变量，那么此条件变量会丢失。

-------------------
#### 管程
由过程变量及数据结构组成的一个集合，它们组成一个特许的模块或软件包

__管程时语言概念__，C语言不支持。

__在任何一个时刻管程只能有一个活跃进程__

进入管程时的互斥由编译器负责。

-------------------
#### 消息传递

-------------------
#### 屏障
一般用于进程组。


---------------------








