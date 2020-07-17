---
title: MySQL触发器
date: 2020-07-17 13:57:00
tags: MySQL 
toc: false
---


# Mysql触发器

### 什么是触发器

触发器是与表有关的数据库对象，在满足定义条件时触发，并执行触发器中定义的语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性。

<!--more-->

### 创建触发器的语法

```mysql
CREATE TRIGGER trigger_name trigger_time trigger_event ON tb_name FOR EACH ROW trigger_stmt;

-- 字段解析
trigger_name：触发器的名称
tirgger_time：触发时机，为BEFORE或者AFTER
trigger_event：触发事件，为INSERT、DELETE或者UPDATE
tb_name：表示建立触发器的表明，就是在哪张表上建立触发器
trigger_stmt：触发器的程序体，可以是一条SQL语句或者是用BEGIN和END包含的多条语句

所以可以说MySQL创建以下六种触发器：
BEFORE INSERT,BEFORE DELETE,BEFORE UPDATE
AFTER INSERT,AFTER DELETE,AFTER UPDATE
```

**创建有多个执行语句的触发器**

```mysql
CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件
ON 表名 FOR EACH ROW
BEGIN
    执行语句列表
END
```

其中，BEGIN与END之间的执行语句列表参数表示需要执行的多个语句，不同语句用分号隔开

**tips：**一般情况下，mysql默认是以 ; 作为结束执行语句，与触发器中需要的分行起冲突

　　  为解决此问题可用DELIMITER，如：DELIMITER ||，可以将结束符号变成||

　　  当触发器创建完成后，可以用DELIMITER ;来将结束符号变成;

```mysql
mysql> DELIMITER ||
mysql> CREATE TRIGGER demo BEFORE DELETE
    -> ON users FOR EACH ROW
    -> BEGIN
    -> INSERT INTO logs VALUES(NOW());
    -> INSERT INTO logs VALUES(NOW());
    -> END
    -> ||
Query OK, 0 rows affected (0.06 sec)
```

**触发器类型**

| 触发器类型     | 激活触发器的语句           |
| :------------- | :------------------------- |
| INSERT型触发器 | INSERT, LOAD DATA, REPLACE |
| Y              | UPDATE                     |
| DELETE型触发器 | DELETE, REPLACE            |

**NEW和OLD的使用**

| 触发器类型     | 激活触发器的语句                                             |
| :------------- | :----------------------------------------------------------- |
| INSERT型触发器 | NEW表示将要或者已经新增的数据                                |
| UPDATE型触发器 | OLD用来表示将要或者已经被删除的语句，NEW表示将要或者已经修改的数据 |
| DELETE型触发器 | OLD表示将要或者已经被删除的数据                              |

### 触发器的应用举例

例如需要存储大量的URL，并需要根据URL进行搜索查找。如果使用B-Tree来存储URL，存储的内容就会很大，因为URL本身都很长。而且对完整的URL字符串做索引会非常慢。正常情况下会有如下查询：

```mysql
SELECT * FROM url WHERE url="https://scooola.github.io";
```

若删除原来URL列上的索引，而新增一个被索引的url_crc列，使用CRC32做哈希，就可以使用下面的方式查询：

```mysql
SELECT * FROM url WHERE url="https://scooola.github.io" AND url_cc=CRC32("https://scooola.github.io");
```

这样做的性能会非常高，因为MySQL优化器会使用这个选择性很高而体积很小的基于url_crc列的索引来完成查找。

这样实现的缺陷是需要维护哈希值。可以手动维护，也可以使用触发器实现。首先创建如下表：

```mysql
mysql> CREATE TABLE pseudohash (
    -> id int NOT NULL auto_increment,
    -> url varchar(255) NOT NULL,
    -> url_crc int NOT NULL DEFAULT 0,
    -> PRIMARY KEY(id)
    -> );
```

**创建触发器**

```mysql
-- 新增数据触发器
CREATE TRIGGER pseudohash_crc_ins INSERT UPDATE ON pseudohash FOR EACH ROW SET NEW.url_crc=crc32(NEW.url);
-- 更新数据触发器
CREATE TRIGGER pseudohash_crc_upd BEFORE UPDATE ON pseudohash FOR EACH ROW SET NEW.url_crc=crc32(NEW.url);
```

接下来验证一下触发器如何维护哈希索引：

```mysql
-- 新增
mysql> INSERT INTO pseudohash (url) VALUES('https://scooola.github.io');
mysql> SELECT * FROM pseudohash;
+----+---------------------------+----------+
| id | url                       | url_crc  |
+----+---------------------------+----------+
|  1 | https://scooola.github.io | 47105892 |
+----+---------------------------+----------+

-- 更新
mysql> UPDATE pseudohash SET url='https://scooola.github.io/' WHERE id=1;
mysql> SELECT * FROM pseudohash;
+----+----------------------------+-----------+
| id | url                        | url_crc   |
+----+----------------------------+-----------+
|  1 | https://scooola.github.io/ | 856602962 |
+----+----------------------------+-----------+
```

如果数据表非常大，CRC32()会出现大量的哈希冲突，可以考虑自己实现一个简单的64位哈希函数。

上述示例中，就用触发器来维护了哈希索引的生成，而避免的程序主动生成触发。
