---
title: 分布式概念
date: 
tags:
- 分布式
---
<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [分布式系统](#分布式系统)
- [缓存一致性协议](#缓存一致性协议)
- [哈希一致性算法](#哈希一致性算法)
- [幂等性](#幂等性)

<!-- /TOC -->
</details>

## 分布式系统

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

* Basically Available(基本可用)
* Soft state(软状态)
* Eventually consistent(最终一致性)

## 缓存一致性协议


## 哈希一致性算法

对key进行哈希运算,从而确定键值对存储在哪个缓存服务器

https://blog.csdn.net/xlgen157387/article/details/57406162


## 幂等性

公式表示为: `F(F(x))=F(x)`

常见幂等:
* select查询天然幂等
* web的GET请求
* 特殊场景下的条件update, 条件delete删除

产生幂等问题的场景:
* 重复提交表单
* 重复发起请求

解决方案:
1. 前端控制可能产生幂等问题的操作, 避免重复操作
2. Post/Redirect/Get模式, 如提交表单后直接跳转至提交成功页面, 不提供重复提交的机会
3. session和表单中各自放入一个标识, 后端检查这两个标识, 满足设置的规则表示首次提交, 否则不处理
4. 操作数据库的场景下, 可借助悲观锁和乐观锁
5. redis分布式锁

