---
title: 手写一个LRU算法
date: 
tags:
- Cache
---
<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [方式1](#方式1)
- [方式2](#方式2)
- [方式3](#方式3)

<!-- /TOC -->
</details>


## 方式1

```Java
public class LRUCache extends AbstractMap {
    @Override
    public Object put(Object key, Object value) {
        return null;
    }

    @Override
    public Set<Entry> entrySet() {
        // super.entrySet();
        return super.keySet();
        // return null;
    }

    @Data
    private static class Node {
        Object value;
        Node   next;
    }
}
```

## 方式2


## 方式3

```Java
public class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private       int size = 0;
    private final int MAX_SIZE;

    public LRUCache(int size) {
        super(size, 0.75f, true);
        MAX_SIZE = size;
    }

    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > MAX_SIZE;
    }
}
```


