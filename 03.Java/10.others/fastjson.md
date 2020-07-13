

```Java
@JSONField(format = "yyyyMMdd HH:mm:ss")
private Date                 startEffectDate;
```

为什么大数据量时, 主键推荐使用uuid而不是自增主键?
32位小写16进制数字组成.
mysql自带UUID()函数
```sql
select REPLACE(uuid(), '-', '');
```

**分布式集群中生成唯一性id方案**
以下假设生成32位不带`-`分割的uuid
方案1: 自增主键

依赖于数据库

方案2: uuid

`UUID.randomUUID().toString().replaceAll("-", "")`

32位太长, 无序, 入库时性能较差

方案3: SnowFlake
