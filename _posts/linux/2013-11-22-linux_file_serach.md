---
layout: post
category : note
title: "Linux 文件查找"
tagline: ""
tags : [Linux]
published: true
---
{% include JB/setup %}

### whereis 命令

```
whereis -[bmsu] 文件名或目录名
```

- -b：二进制
- -m：manual目录下文件
- -s：源文件
- -u：非上述项

### local 命令

```
locate -[ir] KEYWORD
```

- -i：忽略大小写
- -r：正则表达式

### find 命令

```
find [PATH] [OPTION] [ACTION]
```

##### PATH

路径最后必须要有`\``

##### OPTION

1. 时间相关 -atime, -ctime, -mtime, -mmin
    - -mtime n: n天前，这一天内被更改过
    - -mtime +n：n天之前，不含n天被更改过
    - -mtime -n：n天之内，包含n天被更改过
    - -[anewer | mnewer | cnewer] FILE：和文件FILE比较某一类时间
2. 用户相关
    - -user -group：仅查找某一用户/组
    - -nouser -nogruop：所有者/组不存在的文件
3. 权限相关
    - perm +mode 例如：`-perm +775`
4. 其他
    - -name 文件名
    - -prune 排除
    - type TYPE；f:文件，bc:设备，d:目录，l:连接，s:SOCKET，p:FIFO。
    - size [±SIZE]：类似于-mtime用法
    - -maxdepth，限定最大搜索目录；-mindepth，最小...
    - [-not | !]，相反匹配。
    - -empty，空文件。

##### ACTION
1. -print：打印结果，默认行为
2. -delete：删除搜索到的文件
3. -exec：使用方法，`-exec COMMAND {} \;`



#### 示例
- 统计当某目录下cpp文件的行数

```
find /path -name '*.cpp' -exec wc -l {} \;
find /path -name '*.cpp' | xargs wc -l
```


















