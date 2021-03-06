---
layout: post
category : note
title: "MySQL 索引"
tagline: ""
tags : [MySQL, database]

published: true
---
{% include JB/setup %}

索引是一种特殊的数据库结构，可以用来快速查询数据库表特定记录。
索引时创建在表上的，对数据库表中一列或多列的值进行排序的一种结构。

索引的优点时可以提高检索数据的速度，这也是创建索引的最主要的原因。联合查询、分组和排序子句查询时可以显著提高效率。

索引的缺点时创建和维护索引需要耗费时间，且占用一定的物理空间。增加、删除和修改数据的效率都降低了。

MySQL中所有存储引擎对每张表至少支持16个索引，索引长度至少为256字节。

索引的存储类型包括：B型树(InnoDB,MyISAM),哈希索引(MEMORY)


#### MySQL的索引类型包括:

1. 普通索引：可以创建在任何数据类型上，无任何附加条件。其值是否唯一和非空属性由字段本身的完整性约束条件决定。
2. 唯一索引：创建时限制该索引值必须唯一。主键就是一种特殊的唯一性索引。
3. 全文索引：使用FULLTEXT参数设置。只能创建在CHAR、VARCHAR或TEXT类型的字段上。只有MyISAM引擎支持。
4. 单列索引：在表的单个字段上创建索引。单列索引只能根据该字段进行索引。可以时普通索引，也可以时唯一性索引或全文索引。只要保证该索引只对应一个字段即可。
5. 多列索引：在表的多个字段上创建索引。该索引指向创建时对应的多个字段，可以通过这几个字段进行查询；但是只有查询条件中使用了这些字段的第一个字段时，索引才会被使用。
6. 空间索引：使用SPATIAL参数设置。只能建立在空间数据类型上。MySQL的空间数据类型包括：GEOMETRY、POINT、LINESTRING和POLYGON等。目前只有MyISAM存储引擎支持，且索引字段不能为空值。

#### 索引设计的原则：

1. 选择唯一性索引。值唯一，可以快速检索某确定的记录。
2. 为经常需要排序、分组和联合操作的字段建立索引。
3. 为经常作为查询条件的字段建立索引。
4. 限制索引的数目。索引过多一方面会占据过多的磁盘空间，另一方面会降低更新表的时间。
5. 尽量使用数据量少的索引。
6. 尽量使用前缀索引。一般在TEXT和BLOG类型的字段上使用。
7. 删除很少使用或不再使用的索引。

#### 创建索引：
##### 创建表的时候创建

```
CREATE TABLE 表名(
    属性名 数据类型 [完整性约束],
    属性名 数据类型 [完整性约束],
    ...
    [UNIQUE | FULLTEXT | SPATIAL] INDEX|KEY
        [别名] (属性名X [长度]) [ASC|DESC]
);
```

1. 索引类型__UNIQUE__：唯一索引；__FULLTEXT__：全文索引；__SPANTIAL__：空间索引。
2. INDEX和KEY参数用来制定字段为索引，二者选择其中一个即可。
3. '属性名X'制定索引对应的字段的名称，该字段必须是前面定义好的字段。
4. 长度，制定索引的长度，必须时字符串类型才可以使用。
5. ASC，升序；DESC，降序。

##### 在已存在的表上创建index

```
CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX 索引名
    ON 表名 (属性名 [长度] [ASC|DESC]);
```

##### 用ALTER TABLE语句创建索引

```
ALTER TABLE 表名 ADD [UNIQUE|FULLTEXT|SPATIAL] INDEX
    索引名 (属性名 [长度] [ASC|DESC]);
```

#### 删除索引

```
DROP INDEX 索引名 ON 表名;
```
