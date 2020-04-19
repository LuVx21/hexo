<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [集合](#集合)

<!-- /TOC -->
</details>

## 集合

新增以下方法可用于创建不可变集合:
```Java
List.copyOf(Collection<? extends E> coll)
Set.copyOf(Collection<? extends E> coll)
Map.copyOf(Map<? extends K, ? extends V> map)
```

Collectors中新增了以下方法用于创建不可变集合:
```Java
Collectors.toUnmodifiableList()
Collectors.toUnmodifiableSet()
Collectors.toUnmodifiableMap()
```
