

```Java
MultiValueMap<String, String> map = new LinkedMultiValueMap<>();
map.add("title", "foobar");
map.add("desc", "foobar1");
map.add("userid", "foobar2");
map.add("title", "foobar0");
System.out.println(map);
```