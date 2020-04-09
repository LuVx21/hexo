./java.base/java/util/AbstractSet.java
./java.base/java/util/BitSet.java
./java.base/java/util/concurrent/ConcurrentSkipListSet.java
./java.base/java/util/concurrent/CopyOnWriteArraySet.java
./java.base/java/util/EnumSet.java
./java.base/java/util/HashSet.java
./java.base/java/util/JumboEnumSet.java
./java.base/java/util/LinkedHashSet.java
./java.base/java/util/NavigableSet.java
./java.base/java/util/regex/IntHashSet.java
./java.base/java/util/RegularEnumSet.java
./java.base/java/util/Set.java
./java.base/java/util/SortedSet.java
./java.base/java/util/TreeSet.java

./java.base/java/nio/charset
./java.base/java/nio/charset/Charset.java
./java.base/java/nio/charset/CharsetDecoder.java
./java.base/java/nio/charset/CharsetEncoder.java
./java.base/java/nio/charset/IllegalCharsetNameException.java
./java.base/java/nio/charset/spi/CharsetProvider.java
./java.base/java/nio/charset/StandardCharsets.java
./java.base/java/nio/charset/UnsupportedCharsetException.java
./java.base/java/time/OffsetDateTime.java
./java.base/java/time/OffsetTime.java
./java.base/java/time/zone/ZoneOffsetTransition.java
./java.base/java/time/zone/ZoneOffsetTransitionRule.java
./java.base/java/time/ZoneOffset.java

./java.base/jdk/internal/org/objectweb/asm/tree/analysis/SmallSet.java
./java.base/sun/net/ConnectionResetException.java
./java.base/sun/nio/ch/NativeThreadSet.java
./java.base/sun/nio/cs/CharsetMapping.java
./java.base/sun/nio/cs/HistoricallyNamedCharset.java
./java.base/sun/nio/cs/StandardCharsets.java
./java.base/sun/reflect/generics/tree/BaseType.java
./java.base/sun/security/x509/CertAttrSet.java
./java.base/sun/security/x509/CertificatePolicySet.java
./java.base/sun/text/normalizer/BMPSet.java
./java.base/sun/text/normalizer/UnicodeSet.java
./java.base/sun/text/normalizer/UnicodeSetStringSpan.java
./java.base/sun/util/PropertyResourceBundleCharset.java



* Set
    * HashSet:以哈希码决定元素位置的set
    * LinkedHashSet
    * EnumSet:枚举类型专用Set,所有元素都是枚举类型.
    * TreeSet:插入时会自动排序的set,但是如果中途修改元素大小,则不会再修改后重新排序,只会在插入时排序.


# Set

* 元素无序
* 不能重复
* 最多1个空值

| 特性     | HashSet                        | LinkedHashSet                  | TreeSet                                 |
| :------- | :----------------------------- | :----------------------------- | :-------------------------------------- |
| 允许空   |                                |                                |                                         |
| 允许重复 |                                |                                |                                         |
| 有序     | ✘                              | ○                              | ○                                       |
| 线程安全 |                                |                                |                                         |
| 父类     | AbstractSet                    | HashSet                        | AbstractSet                             |
| 接口     | Set<br/>Cloneable,Serializable | Set<br/>Cloneable,Serializable | NavigableSet<br/>Cloneable,Serializable |

> Set不保证插入有序是指Set这个接口的规范, 实现类只要遵循这个规范即可, 也能写出有序的set,如TreeSet