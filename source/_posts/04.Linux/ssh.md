<details>
<summary>点击展开目录</summary>
<!-- TOC -->


<!-- /TOC -->
</details>

`ssh-keygen -t rsa`

RSA也是默认的加密类型. 所以你也可以只输入`ssh-keygen`.

默认的RSA长度是2048位.

如果你非常注重安全，那么可以指定4096位的长度:

`ssh-keygen -b 4096 -t rsa`

可以使用一个密码加密秘钥

将公钥上传到你的服务器

`ssh-copy-id username@remote-server`

ssh config

```
Host luvx
    HostName 192.168.1.1
    User luvx
    IdentityFile ~/.ssh/id_rsa
    Port 22
```


[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
