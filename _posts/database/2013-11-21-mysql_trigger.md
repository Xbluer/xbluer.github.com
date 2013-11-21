---
layout: post
category : note
title: "MySQL 触发器"
tagline: ""
tags : [MySQL, database]
published: true
---
{% include JB/setup %}

触发器是由INSERT、UPDATE和DELTELE等事件来触发某种特定的操作。满足触发器的触发条件时，数据库系统就会执行触发器定义中的程序语句。这样做可以保证某些特定操作的一致性。

#### 创建触发器

##### 创建只有一个执行语句的触发器

```
CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件
    ON 表名 FOR EACH ROW 执行语句
```

1. `BEFORE|AFTER`参数指定了触发器执行的事件，BEFORE指在触发事件执行之前触发语句，AFTER反之。
2. “触发事件”是指触发的条件，包括INSERT、UPDATE和DELETE。
3. `FOR EACH ROW`表示任何一条记录上的操作满足触发事件都会触发该触发器。

##### 创建有多个执行语句的触发器

```
CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件
    ON 表名 FOR EACH ROW
    BEGIN
    ...执行语句列表
    END
    &&
DELIMITER;
```

#### 查看触发器

两种方式

```
SHOW TRIGGERS;
```

无法查询指定的触发器，该语句只能查询所有触发器的信息。

```
SELECT * FROM information_schema.triggers;
```

所有的触发器都定义在information_schema数据库下的trigger表中。

#### 删除触发器

```
DROP TRIGGER 触发器名;
```

如果不再需要某个触发器，一定要将其删除。否则会造成数据不一致。

