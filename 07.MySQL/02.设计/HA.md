<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [MySQL HA](#mysql-ha)

<!-- /TOC -->
</details>

## MySQL HA

服务转移的同时, 还要保证数据的一致性

主备方案: 2台机器, 一个服务, 另一个待机, 机器间通过复制保持数据一致

MHA(Master High Availability)方案:

![1693db24ee5c9267](https://gitee.com/LuVx/img/raw/master/mysql/1693db24ee5c9267.jpg)

MHA Manager会定时探测集群中的master节点, 当master出现故障时, 它可以自动将最新数据的slave提升为新的master, 然后将所有其他的slave重新指向新的master. 整个故障转移过程对应用程序完全透明.

以下为美团的 MHA方案:

![44](https://awps-assets.meituan.net/mit-x/blog-images-bundle-2017/95111bcc.png)

具体的流程大致如下:

1. 从宕机崩溃的master保存二进制日志事件(binlog events);
2. 识别含有最新更新的slave；
3. 应用差异的中继日志(relay log)到其他的slave；
4. 应用从master保存的二进制日志事件(binlog events)；
5. 提升一个slave为新的master；
6. 使其他的slave连接新的master进行复制；
7. 对外服务的ip更改为新master的ip
