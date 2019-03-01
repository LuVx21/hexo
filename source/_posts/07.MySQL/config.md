show variables like 'autocommit';
set autocommit='OFF';


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


[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
