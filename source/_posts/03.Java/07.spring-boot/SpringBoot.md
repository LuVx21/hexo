---
title: 
date: 
tags:
- 
---


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


# Tips

MyBatis控制台输出sql语句

```conf
mybatis.configuration.log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```



[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
