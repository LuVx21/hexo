# 配置

## 安装

```shell
wget https://github.com/alibaba/canal/releases/download/canal-1.1.2/canal.deployer-1.1.2.tar.gz
mkdir -p /usr/local/canal
tar zxvf canal.deployer-1.1.2.tar.gz -C /usr/local/canal
```
conf/example/instance.properties
```conf

```

conf/canal.properties
```conf

```




数据库配置:

```ini
[mysql]
default-character-set=utf8

[client]
port = 3307

[mysqld]
port = 3307
basedir="D:\\ren\\mysql56\\"
datadir="D:\\ren\\mysql56\\data\\"

log-bin=mysql-bin
binlog-format=ROW
server_id=1021
character-set-server=utf8
```

监听设置:
```conf
canal.instance.filter.regex=.*\\..*
canal.instance.filter.black.regex=
```
> `.*\\..*`: 监听所有库的所有表
> `test\..*`: test库的所有表
> `test\.test`: test库内的test表


在配置文件里设置,如conf\example\instance.properties里有2个配置选项
canal.instance.filter.regex = .\.. 这个是白名单,
如test\..*只监听test数据库,
test\.test监控test库里的test表,多个的话用逗号隔开, 但是注意这个配置是无效的,必须在客户端subscribe方法里配置,因为会覆盖
canal.instance.filter.black.regex = 这个是黑名单,
排除库表,配置同上,这个配置有效

具体配置示例参照源码目录里filter部分,canal.instance.defaultDatabaseName = 这个配置项也是无效的,配不配没有关系

# canal_mysql_nosql_sync


https://github.com/liukelin/canal_mysql_nosql_sync




# 参考

0. [Canal Kafka RocketMQ QuickStart](https://github.com/alibaba/canal/wiki/Canal-Kafka-RocketMQ-QuickStart)



