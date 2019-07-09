---
title: 拦截器
date: 2017-10-29
tags:
- Java
- Web
---
<details>
<summary>点击展开目录</summary>
<!-- TOC -->

- [参考](#参考)

<!-- /TOC -->
</details>


还能继承`HandlerInterceptorAdapter`
```Java
@Slf4j
@Component
public class ChanelInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object o) throws Exception {
        log.info("============================拦截器启动==============================");
        request.setAttribute("starttime",System.currentTimeMillis());
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object o, ModelAndView modelAndView) throws Exception {
        log.info("===========================执行处理完毕=============================");
        long starttime = (long) request.getAttribute("starttime");
        request.removeAttribute("starttime");
        long endtime = System.currentTimeMillis();
        log.info("============请求地址："+request.getRequestURI()+"：处理时间：{}",(endtime-starttime)+"ms");
    }

    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {
        log.info("============================拦截器关闭==============================");
    }
}
@Configuration
public class MyInterceptorConfig extends WebMvcConfigurerAdapter{
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new ChanelInterceptor()).addPathPatterns("/**");
    }
}
```

>也可以继承自 WebMvcConfigurerAdapter spring5被废弃, `implements WebMvcConfigurer`


## 参考

[](https://my.oschina.net/bianxin/blog/2876640)
[](https://blog.csdn.net/reggergdsg/article/details/52962774)


[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
