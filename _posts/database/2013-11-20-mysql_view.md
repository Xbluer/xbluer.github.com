---
layout: post
category : note
title: "MySQL 视图"
tagline: ""
tags : [MySQL, database]
published: true
---
{% include JB/setup %}

视图是从一个或多个表中导出来的表，是一种虚拟存在的表。数据库中值保存视图的定义，并没有存放视图的数据。创建视图的目的就是为了方便用户操作数据，同时可以控制数据的访问权限。另外视图还可以提高表的独立性，视图屏蔽了原有表结构变化带来的影响。

#### 创建视图

```
CREATE [ALGORITHM = {UNDEFINED|MERGE|TEMPLABLE}]
    VIEW 视图名 [(属性清单)]
    AS SELECT 语句
    [WITH [CASCADED|LOCAL] CHECK OPTION];
```

1. ALGORITHM可选参数，标示视图选择的算法。UNDEFINED表示MySQL将自动选择；MERGE表示将使用视图的语句与视图定义合并起来，使得视图定义的某一部分取代语句的对应部分；TEMPLABLE表示将视图结果存入临时表，然后使用临时表执行语句。
2. CASCADED可选参数，表示更新视图时要满足所有相关视图和表的条件，该参数为默认值；LOCAL表示更新视图时，要满足该视图本身的定义条件即可。__使用CREATE VIEW创建视图时，最好加上WITH CHECK OPTION参数，并且加上CASCADED参数。如此从视图派生出来的新视图后，更新新视图需要考虑其父视图的约束条件。这中方式比较严格，可以保证数据的安全性。__


#### 查看视图

查看视图基本信息。

```
DESCRIBE 视图名;

SHOW TALBE STATUS LIKE '视图名';
```

查看视图详细信息

```
SHOW CREATE VIEW '视图名';

' 所有视图的定义都在information_schema数据库下的views表
SELECT * FROM information_schema.views
```

#### 修改视图

方法一：CREATE OR REPLACE

```
CREATE OR REPLACE [ALGORITHM = {UNDEFINED|MERGE|TEMPLABLE}]
    VIEW 视图名 [(属性清单)]
    AS SELECT 语句
    [WITH [CASCADED|LOCAL] CHECK OPTION];
```

该语句不仅可以修改视图，而且可以创建视图。

方法二：

```
ALTER [ALGORITHM = {UNDEFINED|MERGE|TEMPLABLE}]
    VIEW 视图名 [(属性清单)]
    AS SELECT 语句
    [WITH [CASCADED|LOCAL] CHECK OPTION];
```
ALTER方法只呢修改已经存在的视图。


#### 更新视图
更新视图是指通过视图插入(INSERT)、更新(UPDATE)和删除(DELETE)表中的数据。因为视图是一张虚拟表，其中美欧数据。通过视图更新时，都是要转换到基本表来更新。更新视图时，只能更新权限范围内的数据，超出范围不能更新。


#### 删除视图

```
DROP VIEW [IF EXISTS] 视图名列表 [RESTRICT|CASCADE]
```
