---
title: MySQL:一些知识点
date: 
tags:
- MySQL
---
<!-- TOC -->

- [MySQL基本类型](#mysql基本类型)
- [修改MySQL端口](#修改mysql端口)

<!-- /TOC -->

key:同时具有constraint和index的作用,即约束本列的数据和优化查询
index:仅具有优化查询的作用

# MySQL基本类型

|类型|说明|
|:---|:---|
|char(size)|最多255个字符.长度固定|
|varchar(size)|最多255个字符.长度不固定|
|tinytext|最大255个字符.长度不固定|
|text|最长65535个字符.长度不固定|
|blob|存储blobs(binary large objects).最大65535字节的数据.长度不固定|
|tinyint(size)|1字节|
|smallint(size)|2字节|
|mediumint(size)|3字节|
|int(size)|4字节|
|bigint(size)|8字节|
|float(size,d)|4字节|
|double(size,d)|8字节|
|decimal(size,d)|存储为字符串的浮点数|
|date()|日期,YYYY-MM-DD|3字节|
|datetime()|日期+时间.YYYY-MM-DD HH:MM:SS|8字节|
|timestamp()|时间戳.YYYY-MM-DD HH:MM:SS|4字节|
|time()|时间.HH:MM:SS|3字节|

[SQL 数据类型](http://www.w3school.com.cn/sql/sql_datatypes.asp)

# 修改MySQL端口

Mac:
```
/Library/LaunchDaemons/com.oracle.oss.mysql.mysqld.plist
<string>--port=3306</string>
```

Linux:
```
vim /etc/my.cnf
```



[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)