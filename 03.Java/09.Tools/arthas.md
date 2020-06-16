<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [Arthas](#arthas)
- [命令](#命令)
- [场景](#场景)
- [执行静态方法](#执行静态方法)
- [获取静态变量](#获取静态变量)
- [运行时替换代码](#运行时替换代码)
- [JVM相关](#jvm相关)

<!-- /TOC -->
</details>

## Arthas

总结一下[Arthas](https://alibaba.github.io/arthas/)的使用场景及例子


java.lang.instrument.Instrumentation
redefineClasses
retransformClasses

BTrace
Arthas


## 命令


## 场景

## 执行静态方法

1.


```bash
sc -d -f com.xinmei.ddp.common.utils.ApplicationContextUtils
// 可行
ognl '@com.xinmei.ddp.common.utils.ApplicationContextUtils@applicationContext.getBean("dashboardTask")' -c 3af49f1c
// 可行 -> 实际效果有点不对
ognl '@com.xinmei.ddp.common.utils.ApplicationContextUtils@applicationContext.getBean("dashboardTask").init()' -c 3af49f1c
```


## 获取静态变量

`getstatic com.xinmei.tdata.dwhtools.task.GetBinlogTask activeThreadSet`
`ognl '@com.xinmei.tdata.dwhtools.task.GetBinlogTask@activeThreadSet' -c 3af49f1c`


## 运行时替换代码



## JVM相关



