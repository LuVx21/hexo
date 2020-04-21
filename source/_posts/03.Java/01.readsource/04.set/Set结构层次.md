<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [特征](#特征)
- [汇总](#汇总)

<!-- /TOC -->
</details>

抽象层:

* `./java.base/java/util/AbstractSet`
* `./java.base/java/util/Set`
* `./java.base/java/util/SortedSet`
* `./java.base/java/util/NavigableSet`

实现:

* `./java.base/java/util/HashSet`
* `./java.base/java/util/LinkedHashSet`
* `./java.base/java/util/TreeSet`
* `./java.base/java/util/concurrent/ConcurrentSkipListSet`
* `./java.base/java/util/concurrent/CopyOnWriteArraySet`
* `./java.base/java/util/EnumSet`
* `./java.base/java/util/JumboEnumSet`
* `./java.base/java/util/RegularEnumSet`

## 特征

* 元素无序(指插入和读取顺序一致), 可实现排序
* 不能重复
* 最多1个空值

## 汇总

* HashSet:以哈希码决定元素位置的set
* LinkedHashSet
* EnumSet:枚举类型专用Set,所有元素都是枚举类型.
* TreeSet:插入时会自动排序的set,但是如果中途修改元素大小,则不会再修改后重新排序,只会在插入时排序.

| 特性     | HashSet                        | LinkedHashSet                  | TreeSet                                 |
| :------- | :----------------------------- | :----------------------------- | :-------------------------------------- |
| 允许空   | ○                              | ○                              | ✘                                       |
| 允许重复 | ✘                              | ✘                              | ✘                                       |
| 有序     | ✘                              | ○                              | ○                                       |
| 线程安全 | ✘                              | ✘                              | ✘                                       |
| 父类     | AbstractSet                    | HashSet                        | AbstractSet                             |
| 接口     | Set<br/>Cloneable,Serializable | Set<br/>Cloneable,Serializable | NavigableSet<br/>Cloneable,Serializable |

> Set不保证插入有序是指Set这个接口的规范, 实现类只要遵循这个规范即可, 也能写出有序的set,如TreeSet