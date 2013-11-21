---
layout: post
category : note
title: "MySQL 数据查询"
tagline: ""
tags : [MySQL, database]
published: true
---
{% include JB/setup %}

#### 基本查询语句

```
SELECT 属性列表
    FROM 表名|视图列表
    [WHERE 条件表达式1]
    [GROUP BY 属性名1 [HAVING 条件表达式2]]
    [ORDER BY 属性名2 [DESC|ASC]]
```

1. 属性列表：要查询的字段名。
2. 表名和视图列表：指定要查询的数据来源，可以有多个。
3. 条件表达式1：查询条件。
4. 属性名1：按此字段中数据进行分组。
5. 条件表达式2：满足该表达式的数据才输出。
6. 属性名2：按此字段中的数据进行排序，DESC|ASC指定排序方式，降升，默认为升序。

#### 查询条件

```
WHERE 条件表达式
```

1. 比较：`=、<、<=、>、>=、!=、<>、!>、!<`
2. 指定范围：`BETWEEN AND`、`NOT BETWEEN AND`
3. 指定集合：`IN、NOT IN`，示例：`[NOT] IN (元素1, 元素2,...,元素n)`
4. 匹配字符：`LIKE、NOT LIKE`
5. 是否为空值：`IS NULL、IS NOT NULL`
6. 多个查询条件：`AND、OR`

##### 消除查询结果中重复记录

```
SELECT DISTINCT 属性名
```

##### 分组查询

```
GROUP BY 属性名 [HAVING 条件表达式][WITH ROLLUP]
```

1. HAVING条件表达式：限制分组后的显示，满足条件的结果将被显示。
2. WITH ROLLUP：在所有记录的最后加上一条记录，显示综合。
3. GROUP BY可以与GROUP_CONCAT()函数一起使用。后者会把每个分组中指定字段值都显示出来。
4. 与集合函数使用，包括COUNT(),SUM(),AVG(),MAX(),MIN()。

```
SELECT sex, GROUP_CONCAT(name) FROM employee GROUP BY sex;
SELECT sex, COUNT(sex) FROM employee GROUP BY sex;
```


##### LIMIT 限制查询结果数量
LIMIT是MySQL中一个特殊的关键字，可以用来指定查询结果显示的初始位置，还可以指定一共显示多少条记录。

```
LIMIT 记录数

SELECT * FROM employee LIMIT 2;
' 仅显示2条记录
```

不指定初始位置，仅限制显示的记录条数。

```
LIMIT 初始位置，记录数

SELECT * FROM employee LIMIT 3, 9;
' 从第3条记录开始显示，一共显示9条
```

指定初始位置和记录条数。

#### 集合函数查询

__集合函数一般都需要和GROUP BY关键字一起使用__

1. COUNT()：统计记录条数，`SELECT id, COUNT(*) FROM employee GROUP BY id`
2. SUM()：计算字段总和，
3. AVG()：字段平均值，
4. MAX、MIN：最大、小值，

#### 连接查询

连接查询是将多个表按某个条件连接起来，从中选取需要的数据。当不同的表中存在表示相同意义的字段时，可以通过该字段来连接这几个表。

##### 内连接
内连接可以查询两个或两个以上的表。当表中相同意义字段的值相等时，就查询出该记录。例如父表中的主键和子表中的外键。

```
SELECT 属性列表[表名1.属性x]
    FROM 表名1,表名2
    WHERE 表名1.属性x = 表名2.属性y
```

__属性列表__中对于两个表中意义相同的字段需要显示则要指明字段所属表。



##### 外连接
外连接查询可以查询多个表。外连接查询也需要通过指定字段来进行连接。当该字段相等时，可以查询出该记录；而且该字段取值不等时也可以查询出来。外连接包括左链接和右连接。

```
SELECT 属性名列表
    FROM 表名1 LEFT|RIGHT JOIN 表名2
    ON 表名1.属性名1 = 表名2.属性名2
```

__ON__：指定连接条件


1. 左连接查询：可以查询出“表1”的所有记录，而“表2”只能查询出匹配的记录。
2. 右连接查询：反之。

#### 子查询

子查询是将一个查询语句嵌套在另一个查询语句中。内层查询语句的查询结果可以为外层查询提供查询条件。即：在特定情况下，一个查询语句的条件需要另一个查询语句来获取。子查询可能包括`IN、NOT IN、ANY、ALL、EXISTS、NOT EXISTS`等关键字，还可能包括比较运算符。


##### 带IN关键字的子查询

一个查询语句的条件可能落在另一个SELECT语句的查询结果中，这个可以通过IN关键字来判断。

```
SELECT * FROM employee
    WHERE d_id IN
        (SELECT d_id FROM department)
' 查询所有d_id在department表中的员工数据
```

##### 带比较运算法的子查询

```
SELECT d_id, d_name FROM department
    WHERE d_id != 
        (SELECT d_id FROM employee
            WHERE age = 24)
' 查询所有没有年龄为24岁的员工的部门
```

##### 带有EXISTS关键字的子查询
EXISTS关键，内层查询语句不返回查询的记录，而是返回一个布尔值。如果内层查询语句查询到满足条件的记录，就返回一个true，否则就返回一个fasle。当返回为true时，外层语句将进行查询；返回为false时，外层查询语句不进行查询或者查询不出任何记录。

```
SELECT * FROM employee
    WHERE age > 24 AND EXISTS
        (SELECT d_name FROM department
            WHERE d_id = 1003);
' 如果部门中有d_id取值为1003的记录，则查询employee表中age大于24的记录
```
