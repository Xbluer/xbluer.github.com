---
layout: post
category : note
title: MySQL TABLE 操作
tagline: ""
tags : [MySQL, database]
data : 2013-11-17
published : true
---
{% include JB/setup %}


### 创建表

```
CREATE TABLE 表名(
    属性名 数据类型 [完整性约束],
    属性名 数据类型 [完整性约束],
    ...
    [完整性约束],
    ...
);
```
__最后一行不要加逗号！！！__

##### 完整性约束:
- PRIMARY KEY：标识为主键。主键标示分为两种：一种为单字段主键，即在定义字段的完整性约束中添加；另一种是多字段主键，需要在属性定义完后添加`PRIMARY KEY(属性名1,属性名2,...属性名n)`
- FOREIGN KEY：标识为外键
- NOT NULL：不能为空
- UNIQUE：标示该属性唯一
- AUTO_INCREMENT:一个表中至多只能有一个字段用AUTO_INCREMENT约束且必须时主键的一部分。被约束字段可以时任意一种整型数据。默认从1开始自增。如果第一条记录设置了初值，则新增的记录就从初值开始自增。
    - DEFAULT:可以设置字段的默认值。当新增的记录没有为该字段赋值时就给该字段赋默认值。

__外键的定义：__
如果表A中有属性字段X,且依赖表B的主键。那么称表B为父表，A为子表，X为A的外键。显然A中X字段的数据类型要与B中对应的主键相同。A中的任一条记录字段X的值要么是表B中已存在的主属性，要么是NULL。

```
CONSTRAINT 外键名 FOREIGN KEY (属性名1.1,属性名1.2,...,属性名1.n)
    REFERENCES 表名(属性名2.1,属性名2.2,...,属性名2.n)
```

##### 查看表的结构：
1. `DESCRIBE 表名`：查看基本定义。可以简写为`DESC 表名`
2. `SHOW CREATE TABLE 表名`：查看表的详细定义


### 修改表

修改表操作用到的命令是`ALTER TABLE;`。可以修改表的表名、字段名、字段数据类型、增加字段、删除字段、修改字段的排列顺序、更改默认存储引擎和删除表的外键约束等。

1. 修改表名`ALTER TABLE 旧表名 RENAME [TO] 新表名;`
2. 修改字段的数据类型`ALTER TABLE 表名 MODIFY 属性名 数据类型;`
3. 修改字段名`ALTER TABLE 表名 CHANGE 旧属性名 新属性名 新数据类型;`。不需要修改数据类型则必须将新数据类型与原来的设置成一样的,可以在修改字段名之前查看一下原表`DESC 表名;`,确保两者完全相同。注意不用修改字段的约束。  
4. 增加字段`ALTER TABLE 表名 ADD 属性名 数据类型 [完整性约束] [FIRST | AFTER 属性名2];`。
    - FIRST：将新增字段设置为表的第一个字段。
    - AFTER 属性名2：将新增字段设在在字段属性名2后。
    - 默认新增字段是表的最后一个字段。
    - 对于数据表而言字段的顺序并不会造成影响，将相关的字段放在一起只是便于理解而已。
5. 删除字段`ALTER TABLE 表名 DROP 属性名;`
6. 修改字段排列顺序`ALTER TABLE 表名 MODIFY 属性名1 数据类型 [FIRST | AFTER 属性名2];`
7. 修改表的存储引擎`ALTER TABLE 表名 ENGINE=存储引擎名;`,存储引擎名可选：InnoDB,MyISAM等。若表中已经存在大量数据，修改存储引擎可能会有意外，最好别修改。
8. 删除表的外键约束`ALTER TABLE 表名 DROP FOREIGN KEY 外键别名;`

MODIFY和CHANGE都可以改变字段的数据类型，不同的是CHANGE可以在改变字段数据类型的同时可以改变字段名。如果要使用CHANGE修改字段类型，则要确保新旧字段名相同。

### 删除表
1. 如果没有关联其他表，则`DROP TABLE 表名;`。删除数据表会丢失所有数据，为了防止意外最好先备份所有的数据然后在删除表。
2. 对于有被其他表的外键约束的，不能直接删除。解决的方法有两个，一是先删除子表在删父表，二是先删除子表外键依赖，再删除父表。第二种方法相对安全。

