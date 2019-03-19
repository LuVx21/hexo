


show variables like 'autocommit';
set autocommit='OFF';


## Binlog

```conf
[mysqld]
log-bin=mysql-bin
binlog-format=ROW
server_id=20001
# log_slave_updates=1 # 监测从库时开启
```

```sql
show variables like '%binlog-format%';
```

## 时区

```sql
show variables like '%time_zone%';
set global time_zone = '+08:00';
set time_zone = '+08:00';
flush privileges;
```

```conf
[mysqld]
default-time-zone = '+08:00'
```

## 区分大小写

```sql
show variables like "%lower_case%";
```

```conf
lower_case_tables_name = 1
```
> 1: 不区分大小写, 表名`user`和`USER`不可以同时存在
> 0: 区分大小写, 表名`user`和`USER`可以同时存在
类似的有变量`lower_case_file_system`表示文件系统是否区分大小写: OFF表示区分大小写, ON表示不区分


# 权限

```sql
grant select on *.* to tdata IDENTIFIED by 'toxrtdata';
```


[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
