<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [Debezium](#debezium)
- [Maxwell](#maxwell)
- [canal](#canal)
    - [canal_mysql_nosql_sync](#canal_mysql_nosql_sync)
- [参考](#参考)

<!-- /TOC -->
</details>

Change Data Capture

## Debezium

![architecture](https://debezium.io/images/debezium-architecture.png)

```bash
docker run -it --rm --name zookeeper -p 2181:2181 -p 2888:2888 -p 3888:3888 debezium/zookeeper:1.2

docker run -it --rm --name kafka -p 9092:9092 \
--link zookeeper:zookeeper debezium/kafka:1.2

docker run -it --rm --name mysql -p 3306:3306 \
-e MYSQL_ROOT_PASSWORD=debezium \
-e MYSQL_USER=mysqluser \
-e MYSQL_PASSWORD=mysqlpw debezium/example-mysql:1.2

docker run -it --rm --name mysqlterm \
--link mysql --rm mysql:5.7 sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'

docker run -it --rm --name connect -p 8083:8083 \
-e GROUP_ID=1 \
-e CONFIG_STORAGE_TOPIC=my_connect_configs \
-e OFFSET_STORAGE_TOPIC=my_connect_offsets \
-e STATUS_STORAGE_TOPIC=my_connect_statuses \
--link zookeeper:zookeeper \
--link kafka:kafka \
--link mysql:mysql \
debezium/connect:1.2
```

## Maxwell

支持应用内嵌入, 依赖为:

```xml
<dependency>
    <groupId>com.zendesk</groupId>
    <artifactId>maxwell</artifactId>
    <version>1.27.1</version>
</dependency>
```

[Github:maxwell](https://github.com/zendesk/maxwell)

其内部解析binlog使用了开源库:

```xml
<dependency>
    <groupId>com.zendesk</groupId>
    <artifactId>mysql-binlog-connector-java</artifactId>
    <version>0.23.2</version>
</dependency>
```

[Github:mysql-binlog-connector-java](https://github.com/osheroff/mysql-binlog-connector-java)

使用例:

```Java
public class App {
    public static final String USER = "root";
    public static final String PASSWORD = "root";
    public static final String HOST = "localhost";
    public static final int PORT = 3306;

    public static void main(String[] args) throws IOException {
        BinaryLogClient client = new BinaryLogClient(HOST, PORT, USER, PASSWORD);
        // EventDeserializer eventDeserializer = new EventDeserializer();
        // eventDeserializer.setCompatibilityMode(
        //         EventDeserializer.CompatibilityMode.DATE_AND_TIME_AS_LONG,
        //         EventDeserializer.CompatibilityMode.CHAR_AND_BINARY_AS_BYTE_ARRAY
        // );
        client.registerEventListener(event -> {
            System.out.println(event);
        });

        client.connect();
    }
}
```

## canal

安装配置阅读官网即可, 很详细的讲解

### canal_mysql_nosql_sync

基于canal 的 mysql 与 redis/memcached/mongodb 的 nosql 数据实时同步方案

[Github:canal_mysql_nosql_sync](https://github.com/liukelin/canal_mysql_nosql_sync)

## 参考

0. [Canal Kafka RocketMQ QuickStart](https://github.com/alibaba/canal/wiki/Canal-Kafka-RocketMQ-QuickStart)
