---
title: 计算机网络体系结构
date: 2016-04-16
tags:
- CS
---
<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [OSI七层结构](#osi七层结构)
- [TCP/IP四层结构](#tcpip四层结构)
- [参考](#参考)

<!-- /TOC -->
</details>

#

协议定义了 网络实体间发送报文和接收报文的格式、顺序以及当传送和接收消息时应采取的行动(规则)。

协议数据单元(PDU)Protocol Data Unit,由 协议控制信息(协议头) 和 数据(SDU) 组成

协议头部中含有完成数据传输所需的控制信息, 比如地址、序号、长度、分段标志、差错控制信息等。

传输层及以下各层的PDU均有各自特定的名称:

* 传输层: 段(Segment)
* 网络层: 分组/包(Packet)
* 数据链路层: 帧(Frame)
* 物理层: 比特(Bit)

数据在源站自上而下递交的过程实际上就是不断封装的过程, 而到达目的地后自下而上递交的过程就是不断拆封的过程

![](https://raw.githubusercontent.com/LuVx21/doc/master/source/_posts/01.CS/img/PDU封装实例.png)

![img](/Users/renxie/OneDrive/Code/doc/source/_posts/99.img/Center.png)

# OSI七层结构

开放系统互连基本参考模型OSI/RM (Open System Interconnection Reference Model)

![](https://raw.githubusercontent.com/LuVx21/doc/master/source/_posts/01.CS/img/典型网络体系结构.png)


# TCP/IP四层结构


**实现UDP可靠传输**

UDP是面向无连接,不可靠的, 若实现可靠传输, 首要的问题就是丢包和包的顺序.

1. 给数据包编号, 接收方按包的顺序接受, 或自动调序.
2. 接收方接到数据包后发送确认信息.接收方接收到不正确的包或包丢失, 要能够重发.



# 参考

[计算机网络体系结构综述（下）](https://blog.csdn.net/justloveyou_/article/details/69612153)