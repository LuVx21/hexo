<details>
<summary>点击展开目录</summary>
<!-- TOC -->


<!-- /TOC -->
</details>

**什么是Apache Kafka?**

Apach Kafka是一款分布式流处理框架, 用于实时构建流处理应用.

它有一个核心的功能广为人知, 即作为企业级的消息引擎被广泛使用

Apache Kafka 一路发展到现在, 已经由最初的`分布式提交日志系统`逐渐演变成了`实时流处理框架`.

主要基于事务日志设计.

**Kafka 的设计时什么样的?**

Kafka 将消息以 topic 为单位进行归纳

将向 Kafka topic 发布消息的程序成为 producers.

将预订 topics 并消费消息的程序成为 consumer.

Kafka 以集群的方式运行, 可以由一个或多个服务组成, 每个服务叫做一个 broker.

producers 通过网络将消息发送到 Kafka 集群, 集群向消费者提供消息

**kafka的message格式是什么样的**

一个Kafka的Message由一个固定长度的header和一个变长的消息体body组成

header部分由一个字节的magic(文件格式)和四个字节的CRC32(用于判断body消息体是否正常)构成.

当magic的值为1的时候, 会在magic和crc32之间多一个字节的数据: attributes(保存一些相关属性, 比如是否压缩, 压缩格式等等);

如果magic的值为0, 那么不存在attributes属性, body是由N个字节构成的一个消息体, 包含了具体的key/value消息.

2:

消息由一个固定长度的头部和可变长度的字节数组组成.

头部包含了一个版本号和 CRC32 校验码.

* 消息长度: 4 bytes (value: 1 + 4 + n)
* 版本号: 1 byte, magic(文件格式)
* CRC 校验码: 4 bytes, 用于判断body消息体是否正常
* 具体的消息: n bytes

---

**2.数据传输的事务定义有哪三种?**

数据传输的事务定义通常有以下三种级别:

(1)最多一次: 消息不会被重复发送, 最多被传输一次, 但也有可能一次不传输

(2)最少一次: 消息不会被漏发送, 最少被传输一次, 但也有可能被重复传输

(3)精确的一次(Exactly once): 消息既不重复也不丢失, 每个消息都仅被传输一次

**3.Kafka 判断一个节点是否还活着有那两个条件?**

(1)节点必须可以维护和 ZooKeeper 的连接, Zookeeper 通过心跳机制检查每个节点的连接

(2)如果节点是个 follower, 它必须能及时的同步 leader 的写操作, 延时不能太久


作者: 摘星不想说话
链接: https://juejin.im/post/6844903889003610119
来源: 掘金
著作权归作者所有.
商业转载请联系作者获得授权, 非商业转载请注明出处.



https://juejin.im/post/6844904191186599950
https://xie.infoq.cn/article/6c879c4c3b52e416f251b2909
https://leisure.wang/procedural-framework/middleware/253.html
http://trumandu.github.io/2019/04/13/Kafka%E9%9D%A2%E8%AF%95%E9%A2%98%E4%B8%8E%E7%AD%94%E6%A1%88%E5%85%A8%E5%A5%97%E6%95%B4%E7%90%86/

http://www.mianshigee.com/tag/Kafka


1.如果副本长时间不在ISR中, 这说明什么?

答案与解析:

如果副本长时间不在ISR中, 这表示Follower副本无法及时跟上Leader副本的进度. 通常情况下, 你需要查看Follower副本所在的Broker与Leader副本的连接情况以及Follower副本所在Broker上的负载情况.

2.请你谈一谈Kafka Producer的acks参数的作用.

答案与解析:

目前, acks参数有三个取值: 0, 1和-1(也可以表示成all).

0表示Producer不会等待Broker端对消息写入的应答. 这个取值对应的Producer延迟最低, 但是存在极大的丢数据的可能性.

1表示Producer等待Leader副本所在Broker对消息写入的应答. 在这种情况下, 只要Leader副本数据不丢失, 消息就不会丢失, 否则依然有丢失数据的可能.

-1表示Producer会等待ISR中所有副本所在Broker对消息写入的应答. 这是最强的消息持久化保障.

3.Kafka中有哪些重要组件?

答案与解析:

Broker——Kafka服务器, 负责各类RPC请求的处理以及消息的持久化.
生产者——负责向Kafka集群生产消息.
消费者——负责从Kafka集群消费消息.
主题——保存消息的逻辑容器, 生产者发送的每条消息都会被发送到某个主题上.
4.简单描述一下消费者组(Consumer Group).

答案与解析:

消费者组是Kafka提供的可扩展且具有容错性的消费者机制. 同一个组内包含若干个消费者或消费者实例(Consumer Instance), 它们共享一个公共的ID, 即Group ID. 组内的所有消费者协调在一起来消费订阅主题的所有分区. 每个分区只能由同一个消费组内的一个消费者实例来消费.

5.Kafka为什么不像Redis和MySQL那样支持读写分离?

答案与解析:

第一, 这和它们的使用场景有关. 对于那种读操作很多而写操作相对不频繁的负载类型而言, 采用读写分离是非常不错的方案——我们可以添加很多Follower横向扩展, 提升读操作性能. 反观Kafka, 它的主要场景还是在消息引擎, 而不是以数据存储的方式对外提供读服务, 通常涉及频繁地生产消息和消费消息, 这不属于典型的读多写少场景. 因此, 读写分离方案在这个场景下并不太适合.

第二, Kafka副本机制使用的是异步消息拉取, 因此存在Leader和Follower之间的不一致性. 如果要采用读写分离, 必然要处理副本滞后引入的一致性问题, 比如如何实现Read-your-writes, 如何保证单调读(Monotonic Reads)以及处理消息因果顺序颠倒的问题. 相反, 如果不采用读写分离, 所有客户端读写请求都只在Leader上处理, 也就没有这些问题了. 当然, 最后的全局消息顺序颠倒的问题在Kafka中依然存在, 常见的解决办法是使用单分区, 其他的方案还有Version Vector, 但是目前Kafka没有提供.

zookeeper 作用:
Broker 注册：Broker 是分布式部署并且之间相互独立, Zookeeper 用来管理注册到集群的所有 Broker 节点.
Topic 注册： 在 Kafka 中, 同一个 Topic 的消息会被分成多个分区并将其分布在多个 Broker 上, 这些分区信息及与 Broker 的对应关系也都是由 Zookeeper 在维护
生产者负载均衡：由于同一个 Topic 消息会被分区并将其分布在多个 Broker 上, 因此, 生产者需要将消息合理地发送到这些分布式的 Broker 上.
消费者负载均衡：与生产者类似, Kafka 中的消费者同样需要进行负载均衡来实现多个消费者合理地从对应的 Broker 服务器上接收消息, 每个消费者分组包含若干消费者, 每条消息都只会发送给分组中的一个消费者, 不同的消费者分组消费自己特定的 Topic 下面的消息, 互不干扰.


https://blog.51cto.com/14745561/2541857