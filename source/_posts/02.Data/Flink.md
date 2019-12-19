<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [配置](#配置)
- [Data Sources](#data-sources)

<!-- /TOC -->
</details>


## 配置

web ui地址;

`http://localhost:8081`

定义上传jar包路径(默认路径下, 服务重启自动删除):

```yml
web.upload.dir: /opt/flink/target/
```

## Data Sources

实现 `SourceFunction` 自定义非并行的 source
实现 `ParallelSourceFunction` 接口或者扩展 `RichParallelSourceFunction` 自定义并行的 source

![](https://dev.tencent.com/u/LuVx21/p/img/git/raw/master/flink_class_Function.png)


[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)

