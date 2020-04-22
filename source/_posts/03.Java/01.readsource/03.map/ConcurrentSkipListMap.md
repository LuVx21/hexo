<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [特性](#特性)
- [源码](#源码)
- [线程安全的实现](#线程安全的实现)
- [阅读](#阅读)

<!-- /TOC -->
</details>

ConcurrentSkipListMap

## 特性

1. k-v均不可为null
2. 和 `ConcurrentHashMap`相比, 多用了存储空间, 但实现了有序数据存储
3. 由于`数据节点层`(最后一层)和`索引层`(最后一层上的所有)组成

## 源码

```Java
// 存储容器, 节点类
static class Index<K,V> {
    final Node<K,V> node;
    final Index<K,V> down;
    volatile Index<K,V> right;
}
static final class HeadIndex<K,V> extends Index<K,V> {
    final int level;
}
static final class Node<K,V> {
    final K key;
    volatile Object value;
    volatile Node<K,V> next;
}
```

新增方法:

```Java
private V doPut(K key, V value, boolean onlyIfAbsent) {
    Node<K,V> z;             // added node
    if (key == null)
        throw new NullPointerException();
    Comparator<? super K> cmp = comparator;
    outer: for (;;) {
        for (Node<K,V> b = findPredecessor(key, cmp), n = b.next;;) {// 根据key找到应该位置的前驱节点,b为前驱节点的后继,b,n都在数据节点层
            if (n != null) {// n为null则表明b为链表的尾节点, 新节点直接关联在b上
                Object v; int c;
                Node<K,V> f = n.next;// 理想情况下, 插入后的顺序应为 b -> key -> n -> f
                if (n != b.next)// 并发场景下, b的后继已经发生改变, 退出内层循环重试
                    break;
                if ((v = n.value) == null) {// 链表中间节点,不再存储数据, 进行删除操作后退出内层循环重试
                    n.helpDelete(b, f);
                    break;
                }
                if (b.value == null || v == n)// b被删除, n是标记节点
                    break;
                if ((c = cpr(cmp, key, n.key)) > 0) {// key排序在n.key之后, 原来选择的前驱节点不再合适, 指针依次后移进行选择
                    b = n;
                    n = f;
                    continue;
                }
                if (c == 0) {// 排序位置相同, 由于不能重复, 可能会发生覆盖
                    if (onlyIfAbsent || n.casValue(v, value)) {// value覆盖掉v, 返回旧值
                        @SuppressWarnings("unchecked") V vv = (V)v;
                        return vv;
                    }
                    break; // 结束内部循环
                }
                // else c < 0; fall through
            }

            z = new Node<K,V>(key, value, n);// 此时能确保b -> key -> n的顺序, 只需要建立关系
            if (!b.casNext(n, z))
                break;         // restart if lost race to append to b
            break outer;
        }
    }

    int rnd = ThreadLocalRandom.nextSecondarySeed();
    if ((rnd & 0x80000001) == 0) { // 是否增加索引层
        int level = 1, max;
        while (((rnd >>>= 1) & 1) != 0)
            ++level;//计算出纵向链表的层级也是索引层的层级
        Index<K,V> idx = null;
        HeadIndex<K,V> h = head;// 每层的都节点对象,和Index节点相比, 有level属性
        if (level <= (max = h.level)) {// 产生的随机level小于现有层数时, 不需要增加索引层, 因此只需创建纵向链表(是否可以叫做层桥)
            for (int i = 1; i <= level; ++i)
                idx = new Index<K,V>(z, idx, null);// 以新增节点由底向上创建了纵向链表(整个链表的数据内容都是z)
        }
        else { // 新的层级数大于现有增加索引层
            level = max + 1; // 仅增加一层索引层
            Index<K,V>[] idxs = (Index<K,V>[])new Index<?,?>[level+1];
            for (int i = 1; i <= level; ++i)
                idxs[i] = idx = new Index<K,V>(z, idx, null);// 以新增节点由底向上创建了纵向链表(整个链表的数据内容都是z)
            for (;;) {
                h = head;
                int oldLevel = h.level;
                if (level <= oldLevel) // 二次判断, 如果不需要增加层级, 则放弃目前为止的内容
                    break;
                HeadIndex<K,V> newh = h;
                Node<K,V> oldbase = h.node;
                for (int j = oldLevel+1; j <= level; ++j)
                    newh = new HeadIndex<K,V>(oldbase, newh, idxs[j], j);// 由底向上更新每层头节点, 新节点直接接在头节点后面
                if (casHead(h, newh)) {// cas更新首层头节点
                    h = newh;
                    idx = idxs[level = oldLevel];// 层桥链表的第二层的那个节点, 首层的节点已经加入了首层中
                    break;
                }
            }
        }
        // 循环作用: 
        splice: for (int insertionLevel = level;;) {// 从insertionLevel层开始往下, 将纵向链表节点接入层中
            int j = h.level;
            for (Index<K,V> q = h, r = q.right, t = idx;;) {// 增加层且首次进入时循环时:首层的q -> r,第二层的t
                if (q == null || t == null)// 节点被删除
                    break splice;
                if (r != null) {// 首层头节点的next存在
                    Node<K,V> n = r.node;
                    int c = cpr(cmp, key, n.key);
                    if (n.value == null) {// 移除已删除的节点
                        if (!q.unlink(r))// 移除失败
                            break;
                        r = q.right;// 重新获取头节点的next
                        continue;;// 重新执行上述的检查
                    }
                    if (c > 0) {// 新节点应该在r之后时, 向右依次移动指针, 找到新节点的应该插入的前驱
                        q = r;
                        r = r.right;
                        continue;
                    }
                }// 此时, 理应有 q -> key -> r的顺序
                if (j == insertionLevel) {
                    if (!q.link(r, t))// t加入到q和r中间(将层桥节点加入该层)
                        break; // restart
                    if (t.node.value == null) {
                        findNode(key);// 节点被删除, 整理索引层和数据节点层后直接退出put操作
                        break splice;
                    }
                    if (--insertionLevel == 0)
                        break splice;// 到达最底层
                }
                if (--j >= insertionLevel && j < level)// level为第二层, 首次循环时j为首层
                    t = t.down;
                q = q.down;// 头节点指针下移, 进入下一层
                r = q.right;// 头节点的右节点随着变更
            }
        }
    }
    return null;
}
```

1. 找到新增节点应在位置的前驱节点
2. 根据需要确定是否增加索引层, 从而决定需要以新增节点创建纵向链表(层桥)
3. 创建层桥节点, 构造纵向链表
4. 将该纵向链表的所有节点加入其对应的索引层

> 新增的首层, 只有两个节点: 头节点和新增的节点

查询方法:

```Java
private V doGet(Object key) {
    if (key == null)
        throw new NullPointerException();
    Comparator<? super K> cmp = comparator;
    outer: for (;;) {
        for (Node<K,V> b = findPredecessor(key, cmp), n = b.next;;) {
            Object v; int c;
            if (n == null)
                break outer;
            Node<K,V> f = n.next;
            if (n != b.next)                // inconsistent read
                break;
            if ((v = n.value) == null) {    // n is deleted
                n.helpDelete(b, f);
                break;
            }
            if (b.value == null || v == n)  // b is deleted
                break;
            if ((c = cpr(cmp, key, n.key)) == 0) {
                @SuppressWarnings("unchecked") V vv = (V)v;
                return vv;
            }
            if (c < 0)
                break outer;
            b = n;
            n = f;
        }
    }
    return null;
}
```

删除方法:

```Java
final V doRemove(Object key, Object value) {
    if (key == null)
        throw new NullPointerException();
    Comparator<? super K> cmp = comparator;
    outer: for (;;) {
        for (Node<K,V> b = findPredecessor(key, cmp), n = b.next;;) {
            Object v; int c;
            if (n == null)
                break outer;
            Node<K,V> f = n.next;
            if (n != b.next)                    // inconsistent read
                break;
            if ((v = n.value) == null) {        // n is deleted
                n.helpDelete(b, f);
                break;
            }
            if (b.value == null || v == n)      // b is deleted
                break;
            if ((c = cpr(cmp, key, n.key)) < 0)
                break outer;
            if (c > 0) {
                b = n;
                n = f;
                continue;
            }
            if (value != null && !value.equals(v))
                break outer;
            if (!n.casValue(v, null))
                break;
            if (!n.appendMarker(f) || !b.casNext(n, f))
                findNode(key);                  // retry via findNode
            else {
                findPredecessor(key, cmp);      // clean index
                if (head.right == null)
                    tryReduceLevel();
            }
            @SuppressWarnings("unchecked") V vv = (V)v;
            return vv;
        }
    }
    return null;
}
```

marker节点的存在保证节点一边删除一边插入数据是安全的

## 线程安全的实现




## 阅读

https://blog.csdn.net/guangcigeyun/article/details/8278349

