---
title: SQL调优
date: 2017-05-02
tags:
- SQL
- 调优
---
<!-- TOC -->

- [调优方案](#调优方案)
    - [初步方案](#初步方案)
    - [一些例子](#一些例子)

<!-- /TOC -->

# 调优方案

## 初步方案

| 方案                                 | 说明                                                         |
| :----------------------------------- | :----------------------------------------------------------- |
| SELECT语句                           | 避免使用 *                                                   |
| 基础表的选择                         | FROM, 从右到左的顺序, 选择记录条数最少的表作为基础表, 交叉表 |
| WHERE子句的顺序                      | 自下而上的顺序解析, 过滤掉最大数量记录的条件写在WHERE子句的末尾 |
| 避免使用HAVING子句                   | 检索出所有记录之后才对结果集进行过滤                         |
| 减少访问数据库的次数                 | 解析SQL, 索引选择, 读数据等操作花费时间                      |
| 合并简单的sql                        |                                                              |
| 避免使用耗费资源的操作               | DISTINCT、UNION、MINUS、INTERSECT、ORDER BY                  |
| 使用COMMIT                           | 释放资源, 回滚段上用于恢复数据的信息, 被程序语句获得的锁, redo log buffer 中的空间等 |
| 避免在索引列上使用NOT                | 停止使用索引转而执行全表扫描                                 |
| 避免在索引列上使用计算               | 不使用索引而使用全表扫描                                     |
| 索引列上避免使用IS NULL和IS NOT NULL |                                                              |


## 一些例子

去重复:
```sql
delete from temp e
where e.rowid > (
    select min(x.rowid)
    from temp1 x
    where x.temp_no = e.temp_no
  )
;
```

减少对表的查询:
```sql
select tab_name
from tables
where (tab_name, db_ver) = (
    select
    tab_name, db_ver
    from tab_columns
    where version = 604
)
;
```
优化GROUP BY:
```sql
-- 低效
select job, avg(age) from temp
group by job having job = 'student' or job = 'manager'
;
-- 高效
select job, avg(age) from temp
where job = 'student' or job = 'manager' group by job
;
```

根据需要用UNION ALL替换UNION
```sql
-- 低效
select user_id, bill_id from user_tab1 where age = '20'
union
select user_id, bill_id from user_tab2 where age = '20'
;
-- 高效
select user_id, bill_id from user_tab1 where age = '20'
union all
select user_id, bill_id from user_tab2 where age = '20'
;
```

> 仅使用于重复数据无影响的场景下

用EXISTS替换DISTINCT:
```sql
-- 低效
select distinct user_id, bill_id from user_tab1 d, user_tab2 e
where d.user_id = e.user_id
;
-- 高效
select user_id, bill_id from user_tab1 d where exists(
    select 1 from user_tab2 e where e.user_id = d.user_id
  )
;
```

用EXISTS替代IN、用NOT EXISTS替代 NOT IN
```sql
-- 低效
select a.* from temp a
where age > 0 and a.id in (
    select id from temp1 where name = 'tom'
  )
;
-- 高效
select a.* from temp a
where age > 0 and exists (
    select 1 from temp1 where a.id = id and name = 'tom'
  )
;
```
用>=替代>
```sql
-- 低效
select * from temp where id > 3
;
-- 高效
select * from temp where id >= 4
;
```

用UNION替换OR(索引列状况较好)
```sql
-- 低效
select * from user_tab1 where user_id = 10 or age = 20
;  
-- 高效
select * from user_tab1 where user_id = 10
union
select * from user_tab1 where age = 20
;
```



01. 必须选择记录条数最少的表作为基础表.
    from 是从前往后检索的，所以要最少记录的表放在最前面
02. 采用自下而上的顺序解析WHERE子句,根据这个原理,表之间的连接必须写在其他WHERE条件之前, 那些可以过滤掉最大数量记录的条件必须写在WHERE子句的末尾.同时在链接的表中能过滤的就应该先进行过滤.
    where是从后往前检索，所以能过滤最多数据的条件应放到最后.
03. SELECT子句中避免使用 `*`
04. 尽量多使用COMMIT
05. 计算记录条数时候，第一快：count(索引列)，第二快：cout(*)
06. 用Where子句替换HAVING子句
07. 通过内部函数提高SQL效率
08. 使用表的别名(Alias)
09. 用EXISTS替代IN
10. 用NOT EXISTS替代NOT IN
11. 用表连接替换EXISTS
12. 用索引提高效率
13. 避免在索引列上使用计算（此条包括在select后面  where后面等任何地方，因为在索引列上计算会导致索引失效）
14. 避免在索引列上使用NOT（在索引列使用not会导致索引失效）
15. 用>=替代>
16. 用UNION替换OR (适用于索引列)
17. 用IN来替换OR
18. 避免在索引列上使用IS NULL和IS NOT NULL
19. 总是使用索引的第一个列
20. 用UNION-ALL 替换UNION ( 如果有可能的话)
21. ORDER BY 子句只在两种严格的条件下使用索引.
22. 避免改变索引列的类型
23. 需要当心的WHERE子句
24. 避免使用耗费资源的操作（带有DISTINCT,UNION,MINUS,INTERSECT,ORDER BY）



explain plan for sql;
SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY());



[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
