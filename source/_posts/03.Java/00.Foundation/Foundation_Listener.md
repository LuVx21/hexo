---
title: Java Listener
date: 2017-10-29
tags:
- Java
- Listener
- Web
---
<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [关于](#关于)
- [实现原理](#实现原理)
- [spring中使用](#spring中使用)
- [servlet中的监听器](#servlet中的监听器)
    - [监听对象的创建和销毁](#监听对象的创建和销毁)
    - [监听对象属性的变化](#监听对象属性的变化)
    - [监听session中javabean的状态](#监听session中javabean的状态)
- [应用场景](#应用场景)
- [参考](#参考)

<!-- /TOC -->
</details>

## 关于


## 实现原理

```Java
/**
* 事件源
*/
@Setter
public static class EventSource {
    private Listener listener;

    private Event click() {
        Event event = new Event(this);
        listener.listen(event);
        return event;
    }

    public void registerLister(Listener listener) {
        this.setListener(listener);
    }
}
/**
* 事件
*/
public static class Event extends EventObject {
    Event(Object source) {
        super(source);
    }
}
/**
* 监听器
*/
public static class Listener implements EventListener {
    private void listen(EventObject obj) {
        EventSource source = (EventSource) obj.getSource();
        System.out.println("监听到事件源:" + source);
    }
}
```

使用
```Java
EventSource source = new EventSource();
source.registerLister(new Listener());
source.click();
```

## spring中使用

`org.springframework.context.ApplicationEvent`
`org.springframework.context.ApplicationListener`

```Java

```

## servlet中的监听器

作用:
    监听web中的域对象(ServletContext,ServletRequest,HttpSession)

监听内容:

1. 监听三个对象的创建和销毁(生命周期)
2. 监听三个对象属性的变化
3. 监听session中javabean的状态

> listener全部是接口

监听三个对象的创建和销毁

* ServletContextListener
* ServletRequestListener
* HttpSessionListener

监听三个对象属性的变化

* ServletContextAttributeListener
* ServletRequestAttributeListener
* HttpSessionAttributeListener

监听session中javabean的状态

* HttpSessionActivationListener(钝化和活化)
* HttpSessionBindingListener(绑定和解绑)

### 监听对象的创建和销毁

ServletContextListener
创建:服务器启动的时候, 会为每一个项目都创建一个servletContext
销毁:服务器关闭的时候, 或者项目被移除的时候

ServletRequestListener
创建:请求来的时候
销毁:响应生成的时候

HttpSessionListener
创建:第一次调用request.getSession的时候或jsp访问的时候创建
销毁: 1. session超时 2. 手动销毁session 3.服务器非正常关闭

### 监听对象属性的变化

添加 替换 删除

### 监听session中javabean的状态

HttpSessionBindingListener: 绑定和解绑
    检测java是否添加到session或者从session中移除

HttpSessionActivationListener: 钝化和活化
    钝化:javabean从session中序列化到磁盘上
    活化:javabean从磁盘上加载到了session中

> javabean需要实现序列化接口

可以通过配置文件修改javabean什么时候钝化(合理分配服务器内存)
    一个web项目,在项目下/meta-info创建一个`context.xml`
        内容如下:
```xml
<Context>
    <!--
        maxIdleSwap    :1分钟 如果session不使用就会序列化到硬盘.
        directory    :ren 序列化到硬盘的文件存放的位置.
    -->
    <Manager className="org.apache.catalina.session.PersistentManager" maxIdleSwap="1">
        <Store className="org.apache.catalina.session.FileStore" directory="ren"/>
    </Manager>
</Context>
```

> 这两个接口需要javabean实现.是让javabean感知到自己的状态

## 应用场景


## 参考

[自定义事件及监听](https://www.cnblogs.com/ashleyboy/p/9566579.html)

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

