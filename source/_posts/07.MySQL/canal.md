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

# 参考

0. [Canal Kafka RocketMQ QuickStart](https://github.com/alibaba/canal/wiki/Canal-Kafka-RocketMQ-QuickStart)



