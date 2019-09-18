<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [分布式Session](#分布式session)

<!-- /TOC -->
</details>

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

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
