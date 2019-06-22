---
title: Spring-事务
date: 2018-11-05
tag:
- Java
- Spring
---

# 事务控制

# 事务传播机制

事务传播行为: 多个事务方法相互调用时, 事务如何在这些方法间传播

|No|行为|说明|
|:---|:---|:---|
|1|propagation_required|当前有事务则加入, 否则新建|
|2|propagation_supports|使用当前事务, 无则以非事务方法执行|
|3|propagation_mandatory|使用当前事务, 无则抛异常|
|4|propagation_requires_new|新建事务, 若存在当前事务, 则将其挂起|
|5|propagation_not_supported|非事务方式执行, 若存在当前事务, 则将其挂起|
|6|propagation_never|非事务方式执行, 若存在当前事务, 则抛出异常|
|7|propagation_nested|当前存在事务, 则在嵌套事务内执行, 无则类似于`propagation_required`|

# 原理

TransactionDefinition


# Q&A

# 参考

1. [Spring事务传播行为详解](https://segmentfault.com/a/1190000013341344)

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
