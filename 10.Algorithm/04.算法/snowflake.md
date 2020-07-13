<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [雪花算法](#雪花算法)
- [其他](#其他)
- [阅读](#阅读)

<!-- /TOC -->
</details>

## 雪花算法

1 + 41 + 5 + 5 + 12 = 64bit

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


## 其他

美团: leaf


## 阅读

https://github.com/twitter-archive/snowflake
https://www.cnblogs.com/relucent/p/4955340.html