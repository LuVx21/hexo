---
title: MySQL:一些知识点
date:
tags:
- MySQL
---
<!-- TOC -->

- [MySQL基本类型](#mysql基本类型)
- [修改MySQL端口](#修改mysql端口)
- [存储引擎](#存储引擎)

<!-- /TOC -->

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
# 定位数据

```sql
-- 既没有主键也没有唯一性索引的表
select
    table_schema,
    table_name
from
    information_schema.tables
where
    (table_schema, table_name) not in (
        select distinct table_schema, table_name
        from information_schema.table_constraints
        where constraint_type in ('primary key', 'unique')
    )
    and table_schema not in ('sys', 'mysql', 'information_schema', 'performance_schema')
```
# 查询后更新自身

TODO

```sql
update user_renxie2 R0
set result =
(SELECT (user_name = password) as result
FROM `user_renxie2` R1
where R0.id1 = R1.id1);
----------------------------------
update user_renxie2
set result = R1.result
from user_renxie2 R0
inner join (
select * from (
SELECT id1 as id1,(user_name = password) as result FROM `user_renxie2`
) a
) R1 on R0.id1 = R1.id1;
---------------------------------
update user_renxie2 R0 inner join (select * from (SELECT id1, (user_name = password) as result FROM `user_renxie2`) a) R1 on R1.id1 = R0.id1
using(result)
set R0.result = R1.result;
```

可用:
```sql
update user_renxie2 R1,
(
	select * from (
		SELECT id1 as id1,(user_name = password) as result FROM `user_renxie2`
	)t
)R2
set R1.result = R2.result
where R1.id1 = R2.id1
;
```

# 比较表是否相同

```sql
select id,title
from (
    select id, title from t1
    union all
    select id,title from t2
) tbl
group by id, title
having count(*) = 1
order by id;
```
或
```sql
select * from (select
(CONCAT(R1.db_name, R1.table_name, R1.pk_field, R1.pk_value)
=
CONCAT(R2.db_name, R2.table_name, R2.pk_field, R2.pk_value))as a
from
user R1 inner join user_1 R2 on R1.db_name = R2.db_name
)t
where t.a <> 1
```

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
