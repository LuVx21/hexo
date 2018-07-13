---
title: Java 基础
date: 2017-10-29
tags:
- Java
---
<!-- TOC -->

- [基本类型](#基本类型)
- [重写和重载](#重写和重载)

<!-- /TOC -->

# 基本类型

8种=4整型+2浮点型+1字符型+1布尔型

| 类型    | 位数 | 默认  | 说明                                          |
| :------ | :--- | :---- | :-------------------------------------------- |
| byte    | 8    | 0     |                                               |
| short   | 16   | 0     |                                               |
| int     | 32   | 0     |                                               |
| long    | 64   | 0L    |                                               |
| float   | 32   | 0.0f  | 单精度,8位有效数字,数量级38                   |
| double  | 64   | 0.0d  | 双精度,17位有效数字,数量级308                 |
| char    | 16   |       | 单一的Unicode字符,范围:\u0000~\uffff(0~65535) |
| boolean | 1    | false |                                               |

> BigInteger 支持任意精度的整数
> BigDecimal 支持任何精度的浮点数

# 重写和重载

Overriding:重写,指重新设计从父类继承来的方法的逻辑
Overloaded:重载,指多个方法的方法名相同,但参数的类型或数量说返回值类型不同

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
