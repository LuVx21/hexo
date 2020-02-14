<details>
<summary>点击展开目录</summary>
<!-- TOC -->

    - [接口](#接口)
- [Collectors](#collectors)
- [Optional](#optional)

<!-- /TOC -->
</details>

## 接口

可以使用 `default` 声明非静态的默认方法(带有方法体)

声明 `static` 方法不能搭配 `default` 使用, 同时需要实现方法

# Collectors



# Optional

Optional.of()
Optional.ofNullable()

Optional(T value),empty(),of(T value),ofNullable(T value)


```Java
//Java8前
if (obj != null) {
    ...;
}

// Java8
Optional.ofNullable(obj).ifPresent(u -> {
    ...;
});

Optional.ofNullable(user)
    .filter(u -> "zhangsan".equals(u.getIntention()))
    .orElseGet(() -> {
        CallLogDto user1 = new CallLogDto();
        user1.setIntention("zhangsan");
        return user1;
    });
```
orElseThrow
orElseGet

> ifPresent???


http://www.importnew.com/10360.html
[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
