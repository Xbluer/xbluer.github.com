---
layout: post
category : lessons
tagline: "SQL about table"
tags : [MySQL, database]
---
{% include JB/setup %}


### 创建表

AUTO_INCREMENT:一个表中至多只能有一个字段用AUTO_INCREMENT约束且必须时主键的一部分。被约束字段可以时任意一种整型数据。默认从1开始自增。如果第一条记录设置了初值，则新增的记录就从初值开始自增。

DEFAULT:可以设置字段的默认值。当新增的记录没有为该字段赋值时就给该字段赋默认值。

查看表的结构：
    1. describe：查看基本定义。可以简写为desc
    2. show create table：查看表的详细定义


### 修改表

修改表操作用到的命令是`alter table`。可以修改表的表名、字段名、字段数据类型、增加字段、删除字段、修改字段的排列顺序、更改默认存储引擎和删除表的外键约束等。

1. 修改表名`ALTER TALBE old_table_name RENAME [TO] new_table_name;`
2. 修改字段的数据类型`ALTER TABLE table_name MODIFY data_name data_type;`
3. 修改字段名`ALTER TABLE table_name CHANGE old_data_name new_data_name new_data_type`。不需要修改数据类型则必须将新数据类型与原来的设置成一样的,可以在修改字段名之前查看一下原表`DESC table_name;`,确保两者完全相同。注意不用修改字段的约束。  
MODIFY和CHANGE都可以改变字段的数据类型，不同的是CHANGE可以在改变字段数据类型的同时可以改变字段名。如果要使用CHANGE修改字段类型，则要确保新旧字段名相同。
4. 增加字段`ALTER TABLE table_name ADD data_name data_type [完整性约束] [FIRST | AFTER data_name2];`。
    - FIRST：将新增字段设置为表的第一个字段。
    - AFTER data_name2：将新增字段设在在字段data_name2后
    - 默认新增字段是表的最后一个字段。

