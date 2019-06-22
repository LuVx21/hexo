---
title: Java8:Lambda
date:
tags:
- Lambda
---

## 函数式接口

有且仅有一个抽象方法的接口, 通常使用`@FunctionalInterface`修饰, 不修饰也没关系,
保证接口中有且仅有一个抽象方法, 可以有任意个默认方法(default)

```Java
@FunctionalInterface
public interface Formula {
    int plusOne(int a);

    default int plusSelf(int a) {
        return a + a;
    }
}
```

Usage:
```Java
// java8前可以实现接口或局部内部类使用, 现在可以使用lambda
Formula formula = (a) -> a + 1;
System.out.println(formula.plusOne(100) + " " + formula.plusSelf(100));
```
> 默认方法不能被重写

## Lambda

### 基础语法

![](https://raw.githubusercontent.com/LuVx21/doc/master/source/_posts/99.img/lambda.jpg)
> 单个参数`()`也可省略

### 变量访问

```Java
// Lambda能访问局部变量, 成员变量, 但要求变量final或等效于final
// Variable used in lambda expression should be final or effectively final
@Test
public void testScopes() {
    String b = "a";
    // b = "b";// 放开编译出错
    Function<String, String> function = from -> from.concat(b);
}
```

### 静态引用

使用一个类的`类名::method`(静态方法)或`对象名::method`(普通方法)或``类名::new``(构造函数)作为函数式接口的抽象方法的方法体

使用::获取方法或者构造函数的引用,既可以是类的也可以是对象的

Usage:
```Java
@FunctionalInterface
public interface Convertable<F, T> {
    T convert(F from);
}
@Getter
@ToString
@NoArgsConstructor
@AllArgsConstructor
public class Refrenced {
    private String name;

    public static String toLowerCase(String s) {
        return s.toLowerCase();
    }

    public String startsWith(String s) {
        return String.valueOf(s.charAt(0));
    }
}
```

Usage:
```Java
Convertable<String, String> converter = Refrenced::toLowerCase;
String converted = converter.convert("Java");

Refrenced something = new Refrenced();
converter = something::toUpperCase;
converted = converter.convert("Java");

Convertable<String, Refrenced> converter2 = Refrenced::new;
Refrenced ref = converter2.convert("Java");

Convertable<Refrenced, String> converter1 = Refrenced::captureName;
converted = converter1.convert(something);
```
> 泛型的第一个参数为入参类型, 第二个为返回值类型
> 如果想要使用`类名::非静态方法`, 则方法不能有参数, 泛型也要修改为类

## jdk自带的函数式接口

位于包`java.util.function`下
```Java
Supplier<User> personSupplier = User::new;
User user = personSupplier.get();
Consumer<String> consumer = r -> System.out.println("Hello, " + r);
consumer.accept("World");
```

**条件语句**

```Java
Predicate<String> predicate = s -> s.length() > 0;
System.out.println("长度>0: " + predicate.test("foo"));
System.out.println("长度<=0: " + predicate.negate().test("foo"));
```
> 配合Optional做参数校验

## jdk中的lambda

**比较器**

```Java
// 自定义
// Comparator<String> comparator = new Comparator<String>() {
//         @Override
//         public int compare(String a, String b) {
//             return a.compareTo(b);
//         }
//     };
// comparator = (String a, String b) -> {
//     return a.compareTo(b);
// }
// comparator = (String a, String b) -> a.compareTo(b);
comparator = (a, b) -> a.compareTo(b);
comparator = Comparator.comparing(String::toString);
/// 逆序有reverseOrder()方法
comparator = Comparator.naturalOrder();
```

**集合操作**
```Java
list.forEach(user -> System.out.println(user));
list.forEach(user -> user.getUserName());
list.forEach(User::getUserName);
map.forEach((k,v) -> System.out.println("key: " + k + " value: " + v));
list.sort(Comparator.comparing(User::getUserName, String.CASE_INSENSITIVE_ORDER));
Collections.sort(list, comparator);
```

**线程创建**
```Java
// jdk8前
Runnable r = new Runnable() {
    @Override
    public void run() {
        method();
    }
}
// jdk8后
Runnable r = () -> method();
```

**文件读取**
```Java
File dir = new File("/home/dir/");
// before
FileFilter directoryFilter = new FileFilter() {
    public boolean accept(File file) {
        return file.isDirectory();
    }
};
// 8
File[] dirs = dir.listFiles(f -> f.isDirectory());
```

[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
