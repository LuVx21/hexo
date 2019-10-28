---
title: MySQL:一些知识点
date:
tags:
- MySQL
---
<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [MySQL基本类型](#mysql基本类型)
    - [数值](#数值)
    - [字符](#字符)
    - [时间](#时间)
    - [空间](#空间)
    - [其他](#其他)
    - [日期操作](#日期操作)
- [修改MySQL端口](#修改mysql端口)
- [存储引擎](#存储引擎)
- [复制表](#复制表)
- [表设计](#表设计)
- [自动记录数据插入修改时间](#自动记录数据插入修改时间)
- [库名,表名长度限制](#库名表名长度限制)
- [导出数据](#导出数据)
- [忽略插入](#忽略插入)
- [插入更新](#插入更新)

<!-- /TOC -->
</details>

# MySQL基本类型

| 类型         | 说明                                                         |
| :----------- | :----------------------------------------------------------- |
| char(n)      | 最多255个字符.长度固定                                       |
| varchar(n)   | 最多255个字符.长度不固定                                     |
| tinytext     | 最大255个字符.长度不固定                                     |
| text         | 最长65535个字符.长度不固定                                   |
| blob         | 存储blobs(binary large objects).最大65535字节的数据.长度不固定 |
| tinyint(n)   | 1字节                                                        |
| smallint(n)  | 2字节                                                        |
| mediumint(n) | 3字节                                                        |
| int(n)       | 4字节                                                        |
| bigint(n)    | 8字节                                                        |
| float(n,d)   | 4字节                                                        |
| double(n,d)  | 8字节                                                        |
| decimal(n,d) | 存储为字符串的浮点数,n+2字节                                 |
| date()       | 日期,YYYY-MM-DD                                              |
| datetime()   | 日期+时间.YYYY-MM-DD HH:MM:SS                                |
| timestamp()  | 时间戳.YYYY-MM-DD HH:MM:SS                                   |
| time()       | 时间.HH:MM:SS                                                |

## 数值

整形

| 类型        | 大小  | 范围 | unsigned范围 | 用途 |
| :---------- | :---- | :--- | :----------- | :--- |
| tinyint     | 1字节 |      |              |      |
| smallint    | 2字节 |      |              |      |
| mediumint   | 3字节 |      |              |      |
| int/integer | 4字节 |      |              |      |
| bigint      | 8字节 |      |              |      |

浮点型
| 类型         | 大小            | 范围 | unsigned范围 | 用途     |
| :----------- | :-------------- | :--- | :----------- | :------- |
| float(m,d)   | 4字节           |      |              | 单精度   |
| double(m,d)  | 8字节           |      |              | 双精度   |
| real         |                 |      |              |          |
| decimal(m,d) | m,d大的值+2字节 |      |              | 精确存储 |
| numeric      |                 |      |              | 精确存储 |

unsigned
zerofill

## 字符

| 类型                  | 大小            | 范围 | unsigned范围 | 用途       |
| :-------------------- | :-------------- | :--- | :----------- | :--------- |
| char                  | 0-255           |      |              |            |
| varchar(n)            | 0-65535         |      |              | n:字符个数 |
| tinyblob/tinytext     | 0-255           |      |              |            |
| blob/text             | 0-65535         |      |              |            |
| mediumblob/mediumtext | 0-16 777 215    |      |              | 24次方     |
| longblob/longtext     | 0-4 294 967 295 |      |              | 32次方     |

## 时间

| 类型      | 大小 | 范围 | unsigned范围 | 用途                  |
| :-------- | :--- | :--- | :----------- | :------------------ |
| date      | 3    |      |              | 2019-03-01          |
| time      | 3    |      |              | 12:25:36            |
| datetime  | 8    |      |1000-01-01 00:00:00.000000~9999-12-31 23:59:59.999999| 2008-12-02 22:06:44 |
| timestamp | 4    |      |1970-01-01 00:00:01.000000~2038-01-19 03:14:07.999999|                     |
| year      | 1    |      |              |                     |


## 空间

| 类型                        | 大小 | 范围 | unsigned范围 | 用途 |
| :-------------------------- | :--- | :--- | :----------- | :--- |
| point/multipoint            |      |      |              |      |
| polygon/multipolygon        |      |      |              |      |
| linestring/multilinestring  |      |      |              |      |
| geometry/geometrycollection |      |      |              |      |


## 其他

| 类型      | 大小 | 范围 | unsigned范围 | 用途 |
| :-------- | :--- | :--- | :----------- | :--- |
| bit       |      |      |              |      |
| enum      |      |      |              |      |
| set       |      |      |              |      |
| binary    |      |      |              |      |
| varbinary |      |      |              |      |


[SQL 数据类型](http://www.w3school.com.cn/sql/sql_datatypes.asp)

数据类型的区别:

**date、datetime和timestamp的区别**

* date保存精度到天, 格式为: YYYY-MM-DD, 如2016-11-07
* datetime和timestamp精度保存到秒, 格式为: YYYY-MM-DD HH:MM:SS, 如: 2016-11-07 10:58:27
* timestamp储存占用4个字节, datetime储存占用8个字节
* timestamp可表示范围:1970-01-01 00:00:00~2038-01-09 03:14:07, datetime支持的范围更宽1000-01-01 00:00:00 ~ 9999-12-31 23:59:59
* 索引速度不同. timestamp更轻量, 索引相对datetime更快

## 日期操作
```sql
select curdate()
select current_date - 1
select current_time
select current_timestamp()
select str_to_date('2019-05-28 01:01:01','%y-%m-%d %h:%i:%s') >= current_date - 1
date(create_time) >= date_sub(curdate(), interval 7 day)
#查询本季度数据
select * from 表名 where quarter(时间字段名 )=quarter(now ());
#查询上季度数据
select * from 表名 where quarter(时间字段名 )=quarter(date_sub(now (),interval 1 quarter));
#查询本年数据
select * from 表名 where  year(时间字段名 )=year(now());
#查询上年数据
select * from 表名 where year (时间字段名 )=year (date_sub (now (),interval 1 year ));
#查询前这周的数据
select * from 表名 where  yearweek(date_sub (时间字段名 ,'%y-%m-%d')) = yearweek(now ());
#查询上周的数据
select * from 表名 where  yearweek(date_sub (时间字段名 ,'%y-%m-%d')) = yearweek(now ())-1;
#查询当前月份的数据
select * from 表名 where  date_sub (时间字段名 ,'%y-%m')=date_sub (now (),'%y-%m')
#查询距离当前现在6个月的数据
select * from 表名 where 时间字段名 between date_sub (now (),interval 6 month) andnow ();
#查询上个月的数据
select * from 表名 where  date_sub (时间字段名 ,'%y-%m')=date_sub (date_sub(curdate(), interval 1 month),'%y-%m')
```
# 修改MySQL端口

Mac:
```bash
/Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
<string>--port=3306</string>
```

Linux:
```bash
vim /etc/my.cnf
```
# 存储引擎


MyISAM:

查询性能好,写操作有欠缺.
数据存储格式:顺序存储
索引的指针:索引btree上的节点是一个指向数据物理位置的指针,所以查找起来很快
锁的策略:MyISAM是表锁,涉及到写操作都是串行的

InnoDB:

支持行锁
数据存储格式:聚集索引.
索引的指针:innodb索引节点存的则是数据的主键,所以需要根据主键二次查找.
锁的策略:InnoDB 有行锁,只有写写操作会阻塞.

两阶段锁定协议（two-phase locking protocol）


# 复制表
```sql
create table if not exists user_new (like user);
create table if not exists user_new select * from user;
```
可以拷贝一个表中其中的一些字段:
```sql
create table newadmin as
(
    select username, password from admin
)
```
可以将新建的表的字段改名:
```sql
create table newadmin as
(
    select id, username as uname, password as pass from admin
)
```
可以拷贝一部分数据:
```sql
create table newadmin as
(
    select * from admin where left(username,1) = 's'
)
```
可以在创建表的同时定义表中的字段信息:
```sql
create table newadmin
(
    id integer not null auto_increment primary key
)
as
(
    select * from admin
)
```
# 表设计

**字段设置default为`Null`好，还是`''`好**

《高性能mysql》中是这么说的：

尽量避免NULL

通常情况下最好指定列为 NOT NULL，除非真的需要存储 NULL 值；mysql表定义时如果没有指定列为NOT NULL，默认都是允许NULL的；

如果查询中包含可为NULL的列，对mysql来说更难优化。因为可为NULL的列，使得索引、索引统计、值比较，都更复杂；

可为NULL的列会使用更多的存储空间，在MYSQL里也需要特殊处理。

当可为NULL的列被索引时，每个索引记录需要一个额外的字节，在MyISAM里甚至还可能导致固定大小的索引（例如只有一个整数列的索引）变成可变大小的索引；

通常，把可为NULL的列改为NOT NULL带来的性能提升比较小，所以调优时没有必要首先修改这种情况，除非确定这会导致问题；

但是如果计划在列上建索引，就应该尽量避免设计成可为NULL的列。当然也有例外，比如InnoDB使用单独的bit存储NULL的值，对稀疏数据有很好的空间效率。这一点不适用于MyISAM。
（稀疏数据：是指很多值都是NULL，少数值是非NULL）

# 自动记录数据插入修改时间

```sql
`create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
`update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间'
```


TiDB是PingCAP公司推出的开源分布式关系型数据库，结合了传统的 RDBMS 和 NoSQL 的最佳特性。TiDB 兼容 MySQL，支持无限的水平扩展，具备强一致性和高可用性。TiDB 的目标是为 OLTP (Online Transactional Processing) 和 OLAP (Online Analytical Processing) 场景提供一站式的解决方案。

# 库名,表名长度限制

| 数据库类型 | 库名 | 表名                       | 字段名    |
| :--------- | :--- | :------------------------- | :-------- |
| MySQL      | 64   | 64个字符                   | 64个字符  |
| SQLSERVER  |      | 128个字符，临时表116个字符 | 128个字符 |
| Oracle     |      | 30个字符                   | 30个字符  |

# 导出数据

```sql
select *
into outfile 'd:\\ren\\odps\\bin\\user.log'
fields terminated by ','
enclosed by '"'
lines terminated by '\n'
from user
;
```

# 忽略插入

# 插入更新

```sql
insert into user(user_id, user_name)
values(1, 'foobar')
on duplicate key update
user_name = values(user_name);
```

TokuDB 适用于数据写多读少，而且数据量比较大
支持在线加减字段，在线创建索引，锁表时间很短


[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
