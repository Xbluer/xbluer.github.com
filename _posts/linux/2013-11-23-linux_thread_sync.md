---
layout: post
category : note
title: "Linux 线程同步"
tagline: ""
tags : [Linux, OS, C/C++]
published: true
---
{% include JB/setup %}


### 线程相关的基本函数

- 创建线程

```
int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
                   void *(*start_routine) (void*), void *arg);
```

pthread_create()函数用于创建线程。新的线程从函数start_routine(void *)开始执行，arg用于传递参数。

- 线程退出

```
void pthread_exit(void *retval);
```

pthread_exit()终止调用线程，同时通过retval返回一个值，此值可以由进程内其他线程通过pthread_join()获取。


- 等待线程退出

```
int pthread_join(pthread_t *thread, void **retval);
```

pthread_join()等待一个由thread指定的线程退出，并通过retval取得返回值。

------------------------------
### 互斥量

```
#include <pthread.h>
pthread_mutex_t; // 三种状态
int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *mutexattr);
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_trylock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
int pthread_mutex_distroy(pthread_mutex_t *mutex);
```

1. pthread_mutex_init 创建一个由mutex指向的互斥量对象。初始化设置有mutexattr指定。互斥量对象有三种类型：fast、recursive和error checking。
2. pthread_mutex_lock 阻塞加锁。
3. pthread_mutex_trylock 非阻塞加锁。
4. pthread_mutex_unlock 解锁
5. pthread_mutex_distroy 


------------------------------
### 读写锁

```
#include<pthread.h>
pthread_rwlock_t ; 三种状态
int pthread_rwlock_init(pthread_rwlock_t *rwlock,
                        const pthread_rwlockattr_t *rwlock_attr);
int pthread_rwlock_rdlock(pthread_rwlock_t *rwlock);
int pthread_rwlock_tryrdlock(pthread_rwlock_t *rwlock);
int pthread_rwlock_wrlock(pthread_rwlock_t *rwlock);
int pthread_rwlock_trywrlock(pthread_rwlock_t *rwlock);
int pthread_rwlock_unlock(pthread_rwlock_t *rwlock);
int pthread_rwlock_destroy(pthread_rwlock_t *rwlock);
```

读写所有两种不同的加锁方式：读锁、写锁。

rdlock：可以有多个线程同时加锁，但只能时读锁。试图加写锁的线程阻塞，另外之后试图加读锁的线程也阻塞，防止写线程一直处于阻塞状态。

wrlock：只能有一个线程对其加锁，其余所有线程都被阻塞。

unlock：如果是一个对其加读锁的线程调用的那么仅仅意味这其自身释放了该锁，其他线程有可能仍然持有读锁，若是此线程是最后一个持有读锁的，则unlock之后该读写锁处于未加锁状态。如果时一个对其加了写锁的线程调用，则直接释放锁，且该读写锁对象处于未加锁状态。

其他tryXXX等和互斥量用法类似。

------------------------------
### 条件变量

```
#include <pthread.h>
pthread_cond_t = PTHREAD_COND_INITIALIZER;
int pthread_cond_init(pthread_cond_t *cond, pthread_condattr *cond_attr);
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
int pthread_cond_timedwait(pthread_cond_t *cond, pthread_mutex_t *mutex,
                           const struct timespec *abstime);
int pthread_cond_signal(pthread_cond_t *cond);
int pthread_cond_broadcast(pthread_cond_t *cond);
int pthread_cond_distroy(pthread_cond_t *cond);
```

条件变量：以无竞争的方式等待特定条件发生，条件由锁变量锁住，然后才能计算。

条件变量必须总是和一个互斥量关联

wait：自动释放mutex，并在条件变量cond上挂起。直到signal或broadcast。

timedwait：和wait类似，但线程仅在cond上挂起一定的时间，超出事件则自动被唤醒。

signal：唤醒一个在cond上挂起的线程。

broadcast：唤醒所有在cond上挂起的线程。

线程从挂起状态离开时，会自动申请mutex加锁，最后由线程解锁。

------------------------------
