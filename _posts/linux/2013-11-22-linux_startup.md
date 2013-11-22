---
layout: post
category : note
title: "Linux 启动流程"
tagline: ""
tags : [Linux, OS]
published: true
---
{% include JB/setup %}

1. 加电，读取ROM中的BIOS程序。
2. BIOS程序首先检查计算机硬件能否满足运行条件。该过程成为“硬件自检”，或加电自检POST(Power-on Self-test)
3. BIOS移交控制权，根据设定好的启动顺序将控制权转交给摸一个外部存储设备。BIOS中可以设定此顺序。读取一个外部设备的第一个扇区，即前512B，若最后两个字节时0X55和0XAA，表名此设备可以用于启动；如果不是，控制权转交给“启动顺序”中下一个设备。
4. 读取可启动设备的第一个扇区(主引导记录MBR)，引导计算机从某一个位置寻找OS。或者是启动管理器grub程序。
5. Grub读入OS内核，并把控制权转交给OS内核。Grub退出。
6. OS内核开始部分工作。创建内核堆栈，识别CPU，计算可用内存，禁用中断，启动内存管理单元，最后调用C语言写出的main函数，开始执行OS主要部分。
7. C语言代码区：启动消息缓冲区，保存启动出现的信息，以便调试。分配内核数据结构，系统自动配置硬件。
8. 运行进程0(idle进程，交换器)，继续初始化系统，配置实时时钟，挂在文件系统，创建init进程pid=1,创建页面守护进程pid=2。
9. 加载开机启动程序(守护进程)。相关目录文件：/init.d,/etc/init.d
10. 检查运行级别
    - 0：关机
    - 1：单用户模式。init fork创建一个shell进程直至其结束。
    - 2-5：多用户模式。
11. 多用户模式下：启动shell初始化脚本，运行开机启动程序。相关目录：/etc/rc。一致性检测，挂载附加文件系统，开启守护进程。
12. 用户登入，三种方式：
    1. 命令行登录：init调用getty程序。
    2. ssh登录：调用sshd程序。
    3. 图形界面登录：调用显示管理设备。gnome为gdm。
13. login shell：
    1. 首先读入/etc/profile文件，对所有用户都有效的配置。
    2. 针对当前用户：依次寻找一下三个文件，找到一个就忽略后面的文件
        - ~/.bash_profile
        - ~/.bash_login
        - ~/.profile
    3. 图形用户只加载/etc/profile和~/.profile。
14. non-login shell：不读取/etc/profile和~/.profile等文件，只读入用户的~/.bashrc文件。一般用户配置环境变量就在这个文件里。～/.profile也会调用~/.bashrc。


---------------------

参考：

1. 阮一峰：[计算机是如何启动的？](http://www.ruanyifeng.com/blog/2013/02/booting.html)
2. 阮一峰：[Linux 的启动流程](http://www.ruanyifeng.com/blog/2013/08/linux_boot_process.html)
3. [现代操作系统](http://book.douban.com/subject/3852290/)
