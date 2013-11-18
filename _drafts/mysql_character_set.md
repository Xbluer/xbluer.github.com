---
layout: post
category : lessons
tagline: "SQL about table"
tags : [MySQL, database]
---
{% include JB/setup %}

#### 修改MySQL的默认字符集为utf8

显示MySQL的字符集

{% highlight shell %}
mysql> show variables like 'char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | latin1                     |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | latin1                     |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
{% endhighlight %}


修改配置文件，注意__字段的细节__
{% highlight shell %}
sudo vim /etc/mysql/my.cnf

[client]字段
default-character-set = utf8

[mysql]字段
default-character-set = utf8

[mysqld]
__character-set-server=utf8__

{% endhighlight %}


再次查看修改后的字符集

{% highlight shell %}
mysql> show variables like 'char%';
+--------------------------+----------------------------+
| Variable_name            | Value                      |
+--------------------------+----------------------------+
| character_set_client     | utf8                       |
| character_set_connection | utf8                       |
| character_set_database   | utf8                       |
| character_set_filesystem | binary                     |
| character_set_results    | utf8                       |
| character_set_server     | utf8                       |
| character_set_system     | utf8                       |
| character_sets_dir       | /usr/share/mysql/charsets/ |
+--------------------------+----------------------------+
8 rows in set (0.00 sec)
{% endhighlight %}

修改完毕
