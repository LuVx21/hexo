


对接口的实现进行动态替换
降低依赖


Service Provider Interfaces

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
