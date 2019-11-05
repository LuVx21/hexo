<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [关于](#关于)
- [使用场景](#使用场景)
- [使用](#使用)

<!-- /TOC -->
</details>

## 关于
SPI: Service Provider Interface, JDK内置的服务提供发现机制

* 对接口的实现进行动态替换
* 降低依赖

## 使用场景

`DriverManager`

找到一个`mysql-connector-java-5.1.44.jar`文件, 解压后在`META-INF/services`目录下有一个名为`java.sql.Driver`的文件

其内容:
```java
com.mysql.jdbc.Driver
com.mysql.fabric.jdbc.FabricMySQLDriver
```

这样的数据库驱动包就不需要使用`Class.forName("com.mysql.jdbc.Driver")`来注册驱动

## 使用

```Java
package org.luvx.service;
public interface Service {
    void run();
}
package org.luvx.service.impl;
public class SimpleService implements Service {
    @Override
    public void run() {
        System.out.println("service loader调用");
    }
}
```
`META-INF/services/`目录下创建`org.luvx.service.Service`文件, 内容为`org.luvx.service.impl.SimpleService`

```Java
ServiceLoader<Service> serviceLoader = ServiceLoader.load(Service.class);
for (Service service : serviceLoader) {
    service.run();
}
```

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
