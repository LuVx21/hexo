---
title: 计算机网络:协议
date: 2016-04-16
tags:
- CS
---
<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [OSI七层结构](#osi七层结构)
- [TCP/IP四层结构](#tcpip四层结构)
- [RPC协议](#rpc协议)
- [QA](#qa)
- [参考](#参考)

<!-- /TOC -->
</details>

#

协议定义了 网络实体间发送报文和接收报文的格式、顺序以及当传送和接收消息时应采取的行动(规则).

协议数据单元(PDU)Protocol Data Unit,由 协议控制信息(协议头) 和 数据(SDU) 组成

协议头部中含有完成数据传输所需的控制信息, 比如地址、序号、长度、分段标志、差错控制信息等.

传输层及以下各层的PDU均有各自特定的名称:

* 传输层: 段(Segment)
* 网络层: 分组/包(Packet)
* 数据链路层: 帧(Frame)
* 物理层: 比特(Bit)

数据在源站自上而下递交的过程实际上就是不断封装的过程, 而到达目的地后自下而上递交的过程就是不断拆封的过程

![](https://dev.tencent.com/u/LuVx21/p/img/git/raw/master/PDU封装实例.png)

![](https://dev.tencent.com/u/LuVx21/p/img/git/raw/master/Center.png)

## OSI七层结构

开放系统互连基本参考模型OSI/RM (Open System Interconnection Reference Model)

![](https://dev.tencent.com/u/LuVx21/p/img/git/raw/master/典型网络体系结构.png)


## TCP/IP四层结构

## RPC协议

## QA

**RPC和HTTP的关系是什么?**
http适用于接口不多、系统与系统交互较少的场景, 具有简单、直接、开发方便等优点

RPC可以通过HTTP协议实现, 也可以通过其他方式实现, 具有网络开销小, 配合注册中心使用, 安全性高等优点

[HTTP和RPC的优缺点](https://segmentfault.com/a/1190000015920678)

**HTTP协议与TCP/IP协议的关系**

HTTP的长连接和短连接本质上是TCP长连接和短连接. HTTP属于应用层协议，在传输层使用TCP协议，在网络层使用IP协议.

IP协议主要解决网络路由和寻址问题，TCP协议主要解决如何在IP层之上可靠的传递数据包，使在网络上的另一端收到发端发出的所有包，并且顺序与发出顺序一致.

TCP有可靠，面向连接的特点.

## 参考

[计算机网络体系结构综述（下）](https://blog.csdn.net/justloveyou_/article/details/69612153)

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
