
某文件的提交记录

某次提交的详情

某次提交的某文件的修改详情


```bash
git config --global core.quotepath false
```


版本库迁移:
```bash
git clone --bare git@github.com:LuVx21/doc
git push --mirror git@github.com:LuVx21/doc1
```

**git push origin与git push -u origin master的区别**

`git push`

如果当前分支与多个主机存在追踪关系, 那么这个时候`-u`选项会指定一个默认主机, 这样后面就可以不加任何参数使用`git push`.

`git push origin`

上面命令表示, 将当前分支推送到origin主机的对应分支.

如果当前分支只有一个追踪分支, 那么主机名都可以省略.

`git push -u origin master`

上面命令将本地的master分支推送到origin主机, 同时指定origin为默认主机, 后面就可以不加任何参数使用git push了.

不带任何参数的git push, 默认只推送当前分支, 这叫做simple方式. 此外, 还有一种matching方式, 会推送所有有对应的远程分支的本地分支. Git 2.0版本之前, 默认采用matching方法, 现在改为默认采用simple方式