<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [JVM调优工具](#jvm调优工具)
    - [jstat](#jstat)
    - [jstack](#jstack)
    - [jmap](#jmap)
    - [jconsole](#jconsole)

<!-- /TOC -->
</details>


MAT


## JVM调优工具

| 工具     | 作用                                                         |
| :-------- | :------------------------------------------------------------ |
| jps      | 进程状态工具,查看正在运行的JVM进程                                |
| jstat    | 统计信息监视工具,实时显示JVM进程中类装载、内存、垃圾收集、JIT编译等数据 |
| jinfo    | 配置信息工具,查询当前运行着的JVM属性和参数的值                  |
| jmap     | 内存映射工具,生成VM的内存转储快照       |
| jhat     | 堆转储快照分析工具,分析使用jmap生成的dump文件                       |
| jstack   | 堆栈跟踪工具,生成当前JVM的所有线程快照,线程快照是虚拟机每一条线程正在执行的方法,目的是定位线程出现长时间停顿的原因. |
| jconsole | jvm监视管理控制台,图像化显示堆栈等使用情况,可以手动进行GC,非常实用 |
| jcmd     |                                                              |

JVM参数:

| 工具                | 作用                                                         |
| :------------------ | :----------------------------------------------------------- |
| -Xmx                | 最大堆内存                                                   |
| -Xms                | 最小堆内存, 通常设置成跟最大堆内存一样,减少GC                |
| -Xmn                | 设置年轻代大小,官方推荐设置为堆的`3/8`                       |
| -Xss                | 指定线程的最大栈空间, 此参数决定了java函数调用的深度, 值越大调用深度越深, 若值太小则容易出栈溢出错误(StackOverflowError) |
| -XX:PermSize        | 指定方法区(永久区)的初始值,默认是物理内存的1/64, 在Java8永久区移除, 代之的是元数据区, 由-XX:MetaspaceSize指定 |
| -XX:MaxPermSize     | 指定方法区的最大值, 默认是物理内存的1/4, 在java8中由-XX:MaxMetaspaceSize指定元数据区的大小 |
| -XX:NewRatio=n      | 年老代与年轻代的比值,-XX:NewRatio=2, 表示年老代与年轻代的比值为2:1 |
| -XX:SurvivorRatio=n | Eden区与一个Survivor区的大小比值,-XX:SurvivorRatio=8表示Eden区与两个Survivor区的大小比值是8:1:1,因为Survivor区有两个(from, to) |

### jstat

```shell
jstat -gc 665
jstat -gcutil 665
```

### jstack


### jmap

```shell
jstat -heap 665
```

### jconsole


1、使用hostname -i 查看远程主机的hostname.
如果显示为 127.0.0.1，需要进行修改(vim /etc/hosts)
修改127.0.0.0.1 为服务器真实的外网ip.

2、被监控的程序运行时给虚拟机添加一些运行的参数
```conf
## 无需认证的远程监控配置
## 其中-Djava.rmi.server.hostname为上一步修改后主机的外网真实ip, 其中监控的端口号可以自己定义，在远程连接的时候会用到
-Dcom.sun.management.jmxremote.port=60001
-Dcom.sun.management.jmxremote.authenticate=false
-Dcom.sun.management.jmxremote.ssl=false
-Djava.rmi.server.hostname=192.168.1.50
## 设置需要密码的远程登陆配置
-Dcom.sun.management.jmxremote.port=60001
-Dcom.sun.management.jmxremote.authenticate=true
-Dcom.sun.management.jmxremote.ssl=false
-Dcom.sun.management.jmxremote.pwd.file= {jmxremote.password}Path
-Djava.rmi.server.hostname=192.168.1.50
```
> 在/usr/lib/jvm/java-8-oracle/jre/lib/management下的jmxremote.access、jmxremote.password.
> jmxremote.access中存储用户名和对应的权限关系.
> jmxremote.password中存储用户名密码.

3、远程连接





[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
