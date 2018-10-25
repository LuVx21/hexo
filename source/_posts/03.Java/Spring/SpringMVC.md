---
title: SpringMVC
date: 2017-10-03
tags:
- Java
- SpringMVC
---

# 处理流程


#

核心组件:

* 处理器映射器
* 处理器适配器
* 视图解析器

# 参数绑定(从请求中接收参数)

1. 默认类型:
    在controller方法中可以有也可以没有,看自己需求随意添加.
    httpservletRqeust,httpServletResponse,httpSession,Model(ModelMap其实就是Mode的一个子类
    ,一般用的不多)
2. 基本类型:string,double,float,integer,long.boolean
3. pojo类型:页面上input框的name属性值必须要等于pojo的属性名称
4. vo类型:页面上input框的name属性值必须要等于vo中的属性.属性.属性....
5. 自定义转换器converter:
    作用:由于springMvc无法将string自动转换成date所以需要自己手动编写类型转换器
    需要编写一个类实现Converter接口
    在springMvc.xml中配置自定义转换器
    在springMvc.xml中将自定义转换器配置到注解驱动上

# 常用注解

`@Controller`
负责注册一个bean 到spring 上下文中.

`@RequestMapping`
注解为控制器指定可以处理哪些 URL 请求.

`@RequestBody`
该注解用于读取Request请求的body部分数据,使用系统默认配置的HttpMessageConverter进行解析,然后把相应的数据绑定到要返回的对象上 ,再把HttpMessageConverter返回的对象数据绑定到 controller中方法的参数上.

`@ResponseBody`
该注解用于将Controller的方法返回的对象,通过适当的HttpMessageConverter转换为指定格式后,写入到Response对象的body数据区.

`@ModelAttribute`
* 在方法定义上使用:Spring MVC 在调用目标处理方法前,会先逐个调用在方法上标注了@ModelAttribute 的方法.
* 在方法的形参前使用:可以从隐含对象中获取隐含的模型数据中获取对象,再将请求参数–绑定到对象中,再传入入参将方法入参对象添加到模型中.

`@RequestParam`
在处理方法入参处使用 该注解 可以把请求参数传递给请求方法.

`@PathVariable`
绑定 URL 占位符到入参.

`@ExceptionHandler`
注解到方法上,出现异常时会执行该方法.

`@ControllerAdvice`
使一个Contoller成为全局的异常处理类,类中用@ExceptionHandler方法注解的方法可以处理所有Controller发生的异常.

# 执行流程

![](https://raw.githubusercontent.com/LuVx21/doc/master/source/_posts/99.img/SpringMVC_Flow.png)

图片内容引用自[springMVC执行流程及原理](https://blog.csdn.net/liangzi_lucky/article/details/52459378)

1. 所有的请求都提交给DispatcherServlet
2. DispatcherServlet查询HandlerMapping, 找到处理请求的Controller
3. DispatcherServlet将请求提交到目标Controller
4. Controller进行业务处理, 返回一个ModelAndView
5. Dispathcher查询ViewResolver视图解析器, 找到ModelAndView对象指定的视图对象
6. 视图对象进行渲染并返回给客户端

1. 用户向服务器发送请求, 请求被SpringMVC的前端控制器DispatcherServlet截获.
2. DispatcherServlet对请求的URL（统一资源定位符）进行解析, 得到URI(请求资源标识符), 然后根据该URI, 调用HandlerMapping获得该Handler配置的所有相关的对象, 包括Handler对象以及Handler对象对应的拦截器, 这些对象都会被封装到一个HandlerExecutionChain对象当中返回.
3. DispatcherServlet根据获得的Handler, 选择一个合适的HandlerAdapter. HandlerAdapter的设计符合面向对象中的单一职责原则, 代码结构清晰, 便于维护, 最为重要的是, 代码的可复制性高.HandlerAdapter会被用于处理多种Handler, 调用Handler实际处理请求的方法.
4. 

提取请求中的模型数据, 开始执行Handler(Controller). 在填充Handler的入参过程中, 根据配置spring将帮助做一些额外的工作
消息转换: 将请求的消息, 如json, xml等数据转换成一个对象, 将对象转换为指定的响应信息.
数据转换: 对请求消息进行数据转换, 如String转换成Integer. Double等.
数据格式化: 对请求的消息进行数据格式化, 如将字符串转换为格式化数字或格式化日期等.
数据验证: 验证数据的有效性如长度, 格式等, 验证结果存储到BindingResult或Error中.

5. Handler执行完成后, 向DispatcherServlet返回一个ModelAndView对象, ModelAndView对象中应该包含视图名或视图模型.
6. 根据返回的ModelAndView对象, 选择一个合适的ViewResolver(视图解析器)返回给DispatcherServlet.
7. ViewResolver结合Model和View来渲染视图.
8. 将视图渲染结果返回给客户端.

以上8个步骤, DispatcherServlet. HandlerMapping. HandlerAdapter和ViewResolver等对象协同工作, 完成SpringMVC请求—>响应的整个工作流程, 这些对象完成的工作对于开发者来说都是不可见的, 开发者并不需要关心这些对象是如何工作的, 开发者, 只需要在Handler(Controller)当中完成对请求的业务处理.



[![](https://static.segmentfault.com/v-5b1df2a7/global/img/creativecommons-cc.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
