<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [HTTP Client](#http-client)
- [String](#string)

<!-- /TOC -->
</details>

## HTTP Client

在Java9, Java10 开始孵化, 11中成为正式模块, 命名为`java.net.http`模块

```Java
public static void sync() throws URISyntaxException, IOException, InterruptedException {
    HttpClient client = HttpClient.newHttpClient();
    HttpRequest request = HttpRequest.newBuilder()
            .uri(new URI("http://openjdk.java.net/"))
            .GET()
            .build();

    HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
    System.out.println(response.headers());
    System.out.println(response.statusCode());
    System.out.println(response.body());
}

public static void async() throws URISyntaxException {
    HttpClient client = HttpClient.newHttpClient();
    HttpRequest request = HttpRequest.newBuilder()
            .uri(new URI("http://openjdk.java.net/"))
            .GET()
            .build();

    CompletableFuture<HttpResponse<String>> response = client.sendAsync(request, HttpResponse.BodyHandlers.ofString());
    response
            .whenComplete((resp, t) -> {
                if (t != null) {
                    t.printStackTrace();
                } else {
                    System.out.println(resp.body());
                    System.out.println(resp.statusCode());
                }
            })
            .join();
}
```

## String

```Java
// 是否为空白字符串
public boolean isBlank()
//去除首尾空格
public String strip()
//去除首部空格
public String stripLeading()
//去除尾部空格
public String stripTrailing()
// 重复字符串
public String repeat(int count)
// 返回由换行符分隔的字符串集合
public Stream<String> lines()
```