

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

UUID.randomUUID().toString().replaceAll("-", "")

32位太长, 无序, 入库时性能较差

方案3: SnowFlake

1 + 41 + 10 + 12 = 64bit

1. 1位始终为0, 无实际作用
2. 41位时间戳
3. 10位机器id, 高5bit是数据中心ID(datacenterId), 低5bit是工作节点ID(workerId), 最多可以容纳1024个节点
4. 12位序列号, 这个值在同一毫秒同一节点上从0开始不断累加, 最多可以累加到4095

优点:
1. 生成ID时不依赖于DB，完全在内存生成，高性能高可用。
2. ID呈趋势递增，后续插入索引树的时候性能较好。
缺点:
依赖于系统时钟的一致性


因此同一毫秒内, 最多可以生成 1024 X 4096 = 4194304 个唯一id

https://github.com/twitter-archive/snowflake