---
title: 分布式缓存
date: 
tags:
- Web
---
<!-- TOC -->

- [分布式系统](#分布式系统)
- [缓存一致性协议](#缓存一致性协议)
    - [哈希一致性算法](#哈希一致性算法)
- [分布式Session](#分布式session)
- [分布式事务](#分布式事务)
- [分布式锁](#分布式锁)

<!-- /TOC -->

# 分布式系统

分布式系统中删除,增加所带来的影响及解决方案

clustered cache
distributed cache
性能
一致性

consistency
scalability
infrastructure

cache buffer


负载均衡方案中有一种就是IP Hash,即针对访问者IP进行Hash运算,从而决定由那台服务器为该用户提供服务.

CAP理论

WEB服务无法同时满足以下3个属性:

* 一致性(Consistency)
* 可用性(Availability)
* 分区容错性(Partition tolerance)

BASE理论

* Basically Available（基本可用）
* Soft state（软状态）
* Eventually consistent（最终一致性）

# 缓存一致性协议


## 哈希一致性算法

对key进行哈希运算,从而确定键值对存储在哪个缓存服务器

# 分布式Session

Session一致性解决方案:

* 客户端存储Session,不常用
* 同步
* 保证请求分配到同一服务器
* 后端统一存储,推荐存在缓存中

```xml
<context:annotation-config/>
<bean class="org.springframework.session.data.redis.config.annotation.web.http.RedisHttpSessionConfiguration"/>
<bean class="org.springframework.data.redis.connection.lettuce.LettuceConnectionFactory"/>
```

https://blog.csdn.net/wojiaolinaaa/article/details/62424642

https://blog.csdn.net/xlgen157387/article/details/57406162

# 分布式事务

https://www.cnblogs.com/savorboard/p/distributed-system-transaction-consistency.html


# 分布式锁

[基于redis的分布式锁实现](https://juejin.im/entry/5a502ac2518825732b19a595)

方案1: setnx + expire 命令, value为是谁加的锁, 但也可能出问题, 参考[Redis RedLock 完美的分布式锁么？](https://www.xilidou.com/2017/10/29/Redis-RedLock-%E5%AE%8C%E7%BE%8E%E7%9A%84%E5%88%86%E5%B8%83%E5%BC%8F%E9%94%81%E4%B9%88%EF%BC%9F/)
方案2: setnx命令, value为currenttime + timeouttime

加锁:
```Java
private static final String LOCK_SUCCESS = "OK";
private static final String SET_IF_NOT_EXIST = "NX";
private static final String SET_WITH_EXPIRE_TIME = "PX";

/**

* 尝试获取分布式锁
* @param jedis Redis客户端
* @param lockKey 锁
* @param requestId 请求标识
* @param expireTime 超期时间
* @return 是否获取成功
*/
public static boolean tryGetDistributedLock(Jedis jedis, String lockKey, String requestId, int expireTime) {
    String result = jedis.set(lockKey, requestId, SET_IF_NOT_EXIST, SET_WITH_EXPIRE_TIME, expireTime);

    if (LOCK_SUCCESS.equals(result)) {
        return true;
    }
    return false;
}
```

解锁:
```Java
private static final Long RELEASE_SUCCESS = 1L;

/**

* 释放分布式锁
* @param jedis Redis客户端
* @param lockKey 锁
* @param requestId 请求标识
* @return 是否释放成功
*/
public static boolean releaseDistributedLock(Jedis jedis, String lockKey, String requestId) {
    String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
    Object result = jedis.eval(script, Collections.singletonList(lockKey), Collections.singletonList(requestId));
    if (RELEASE_SUCCESS.equals(result)) {
        return true;
    }
    return false;
}
```



[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

