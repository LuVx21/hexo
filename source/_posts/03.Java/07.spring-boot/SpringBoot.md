---
title:
date:
tags:
-
---


## 打包

## 启动

##　关闭

pom.xml:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

application.properties:
```conf
#启用shutdown
endpoints.shutdown.enabled=true
#禁用密码验证
endpoints.shutdown.sensitive=false
```

curl -X POST http://localhost:8080/shutdown

> https://blog.csdn.net/quincuntial/article/details/54410916

mvn package -P prod
mvn spring-boot:run -Dspring.profiles.active=dev

# 注解

## @Value

```conf
org.luvx.title=LuVx
org.luvx.description=Sample of Spring-Boot
```
```Java
@Component
public class Ren {
    @Value("${org.luvx.title}")
    private String title;

    private static String description;

    @Value("${org.luvx.description}")
    public void setDescription(String description) {
        Ren.description = description;
    }

    @PostConstruct
    public void run() {
        System.out.println(title);
        System.out.println(description);
    }
}
```

## @Bean

## @PostConstruct


## @Configuration

classpath:/config


## @ConfigurationProperties

## @RequestParam

## @PathVariable

## @RequestBody

## @Profile


## @Primary/@Qualifier
当你一个接口的实现类有多个的时候，你通过@Component来注册你的实现类有多个，但是在注入的时候使用@Autowired
@Primary	优先方案，被注解的实现，优先被注入
@Qualifier	先声明后使用，相当于多个实现起多个不同的名字，注入时候告诉我你要注入哪个


# Tips

MyBatis控制台输出sql语句

```conf
mybatis.configuration.log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```





[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
