---
layout: post
category : note
title: "C语言内存分配"
tagline: ""
tags : [C/C++, Linux]
published: true
---
{% include JB/setup %}

-----------------------
### malloc
函数原型：

```
#include <stdlib.h>
void *malloc(size_t size);
```

C语言库函数，在堆中分配内存。数不会初始化，要用`free()`回收果。sizeo==0,则返回NULL或一个独一无二的地址用于释放。

-----------------------
### calloc
函数原型：

```
#include <stdlib.h>
void *calloc(size_t nmemb, size_t size);
```

C语言库函数，堆，__分配的空间会被初始化为0__。分配的内存大小为nmemb*size。参数中任意一个为0,则与`malloc(0)`行为类似。

-----------------------
### realloc
函数原型：

```
#include <stdlib.h>
void realloc(void *ptr, size_t size)
```

C语言库函数，堆。将ptr所指的内存空间修改为size字节，可以扩大也可以缩小，返回新分配的内存地址。如果时扩大内存，系统优先在原为扩展返回值就是ptr，如果失败则重新分配内存并将原来内存中的数据拷贝到新分配的内存，新增加的内存没有被初始化。

如果size==0且ptr!=NULL，则相当于`free(ptr)`。

若ptr==NULL，则相当于`malloc(size)`。

-----------------------
### alloca
函数原型：

```
#include <alloca.h>
void alloca(size_t size);
```

在栈上分配内存。__调用函数退出时会自动释放内存，不用free__，初始化内存。

具体行为依赖于机器和编译器。有可能导致栈溢出，引发未定义行为。

-----------------------
### brk、sbrk
函数原型：

```
#include <unistd.h>
int brk(void *addr);
void *sbrk(intptr_t increment);
```

二者都是修改进程的数据段(data segment)大小。

1. brk 通过`addr`设置数据段结束地址。addr的值必须是合理的，系统要有足够的内存，且进程不会超过数据段最大限制。成功返回0,错误返回-1且设置errno为ENOMEM。

2. sbrk将数据段大小增加increment字节。成功，则返回原先数据段的结束地址，即新分配的内存的起始地址；错误，返回(void *)-1，同时捡errno设置为ENOMEM。

将increment设置为0，可以得到当前系统数据段结束地址。

-----------------------
### mmap 文件内存映射
函数原型：

```
#include <sys/mman.h>
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
int munmap(void* addr, size_t length);
```

mmap将一个文件或者其它对象映射进内存。此函数功能较多。对于内存分配，进程可以进行匿名映射方式申请内存而不用与特定的文件绑定。munmap执行相反的操作，删除特定地址区域的对象映射。

- addr：设置映射地址，若为NULL则系统自动选择；非NULL，Linux系统选择最近的页地址。被分配的内存起始地址会被返回。
- length：映射的内存长度。
- prot：设置权限，读写执行、不可访问。[PROT\_READ | PROT\_WRITE | PROT\_EXEC | PROT\_NONE]
- falgs：内存映射方式。对于此处设置为`MAP_PRIVATE|MAP_ANONYMOUS`，私有的匿名映射。
- fd：被映射文件的文件描述符。此处被忽略。(部分实现要求该值为-1)
- offset：被映射文件的偏移位置。此处被忽略。

返回值：mmap：成功则返回分配的内存地址，失败返回(void*)-1；munmap成功返回0，错误返回-1且设置errno为EINVAL。

```
p = mmap(NULL, length, PROT\_READ|PROT\_WRITE, -1, 0);
...
if (mummap(p, length) != 0)
    do_error();
```

### malloc 实现

malloc库函数是调用brk|sbrk和mmap来实现的。

一般情况下，malloc在堆上分配内存。堆的大小根据需要调用sbrk来进行调整。

但时当要分配的内存大小超过MMAP\_\_HRESHOLD字节时，malloc利用mmap进行私有匿名映射实现内存分配。MMAP\_\_HRESHOLD默认为128K，可以通过mallopt()调整。
