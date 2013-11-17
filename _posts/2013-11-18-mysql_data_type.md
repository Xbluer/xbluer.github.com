---
layout: post
category : lessons
tagline: "学习笔记"
tags : [MySQL]
---
{% include JB/setup %}

## MySQL的数据类型有四大类分别为：
1. 数值类型：整型,浮点型,定点型
2. 日期与时间类型
3. 字符型
4. 二进制类型


### 整型
类型,显示长度,字节数,说明
1. TINYINT,4,1B：类似与C语言中的byte类型
2. SMALLINT,6,2B：类似于C语言中的short类型
3. MEDIUMINT, 9,3B：
4. INT／INTEGER,11,4B：类似与C中的int型
5. BIGINT,20,8B：长整型
如果使用整型时添加__zerofill__参数,数据中位数不足显示长度会添加0填充。  
__AUTO_INCREMENT__属性使得该整形字段成为自增字段,一个表中之多只有一个且必须是主键的一部分。

### 浮点数 ＆ 定点数
1. FLOAT,4B：
2. DOUBLE, 8B：
3. DECIMAL(M, D)/DEC(M, D), M+2：取值范围和double相同,但是大小不相同,默认M=10,D=0。M为精度即数据总长度,D为标度小数点后的长度。  
如果定义的数据的精度高于实际定义的精度,则系统会进行四舍五入,FLOAT和DOUBLE型数据四舍五入时不会报错,但是DEC型会报错。  
MySQL中的定点数是以字符串的形式存储,精度比浮点数高且不会有误差。如果对数据的精度要求较高应该选择DEC。

### 日期与时间
1. YEAR,1B,1901～2155,0000
2. DATE,4B, 
3. TIME, 3B,
4. DATETIME, 8B,
5. TIMESTAMP, 4B,

分隔符,日期`-`,时间`:`

### 字符串
1. char型
    1. CHAR(n),长度范围：0～255,一个字节用于记录长度。长度固定
    2. VARCHAR(n),长度范围：0～65535,2B记录长度长度可变。

2. TEXT,只能保存字符类型,(类型,记录长度字节大小,存储空间)
    1. TINYTEXT, 1B,长度+2B
    2. TEXT, 2B,长度+2B
    3. MEDIUMTEXT,3B,长度+3B
    4. LONGTEXT,4B,长度+4B

3. ENUM ,枚举类型
如果ENUM有not null属性,则默认值为列表第一个元素,否则默认为NULL

4. SET,类型
列表中的元素都有一个排列编号。MySQL保存的是编号而不是元素。  
一个set的值最多包含64个元素。插入的顺序无关紧要,会自动按定义的顺序显示。

### 二进制类型
1. BINARY,VARBINARY
    - BINARY(M),固定长度,
    - VARBINARY(M),长度可变,占用空间比长度多1B
2. BIT(M),M最大为64
3. BOLB类型,可变长二进制数据,
    1. TINYBLOB,1B(保存长度的值大小)
    2. BLOB,2B
    3. MEDIUMBLOB,3B
    4. LONGBLOB,4B

#### 常见问题
1. 路径中有`\\`时用两个`\\\\`保存,不然会被过滤掉
2. SQL中的BOOL和BOOLEAN类型在MySQL中用TINYINT代替
3. 对于图像或者mp3文件,数据库中一般保存其路径,但如果需要保存其自身则保存为BOLB类型数据。
