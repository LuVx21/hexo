


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

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
