<details>
<summary>点击展开目录</summary>
<!-- TOC -->
<!-- /TOC -->
</details>


删除对windows环境变量的引用:

```bash
export PATH=`echo $PATH | sed 's/:\/mnt\/[a-z]\/[^:]*//g'`
```


