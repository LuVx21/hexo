

# 函数式接口

`@FunctionalInterface` 不添加也是正确的, 保证接口中有且仅有一个抽象方法声明即可, 可以有任意默认方法(default)

```Java
@FunctionalInterface
public interface Convertable<F, T> {
    T convert(F from);
}
```

Usage:
```Java
// Convertable<String, Integer> converter = (from) -> Integer.valueOf(from);
Convertable<String, Integer> converter = Integer::valueOf;
Integer converted = converter.convert("123");
System.out.println(converted);
```

# 静态引用

`类名::method`(静态方法)或`对象名.method`(普通方法)或``类名::new``(构造函数)
Usage:
```Java
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
// 创建对象
Convertable<String, Refrenced> constructor = Refrenced::new;
Refrenced ref = constructor.convert("Java");
```
```Java
Refrenced something = new Refrenced();
// 普通方法
Convertable<String, String> converter = something::startsWith;
String converted = converter.convert("Java");
// 静态方法
converter = Refrenced::toLowerCase;
converted = converter.convert("Java");
```


# Lambda

*普通实现*

```Java
File dir = new File("/an/dir/");
   FileFilter directoryFilter = new FileFilter() {
      public boolean accept(File file) {
         return file.isDirectory();
      }
};
```
	
*Lambda实现*

```Java
File dir = new File("/an/dir/");  
File[] dirs = dir.listFiles((File f) -> f.isDirectory());
```

## 变量访问

```Java
static int outerStaticNum = 0;
int outerNum = 0;

@Test
public void testScopes() {
      int aa = 233;

      // 访问局部变量
      Convertable<Integer, String> stringConverter0 = (from) -> String.valueOf(from + aa);

      // 访问成员普通变量
      Convertable<Integer, String> stringConverter1 = (from) -> {
      outerNum = 23;
      return String.valueOf(from);
      };

      // 访问成员静态变量
      Convertable<Integer, String> stringConverter2 = (from) -> {
      outerStaticNum = 72;
      return String.valueOf(from);
      };

      System.out.println(stringConverter0.convert(1));
      stringConverter1.convert(23);
      stringConverter2.convert(24);
      System.out.println(outerNum);
      System.out.println(outerStaticNum);
}
```

