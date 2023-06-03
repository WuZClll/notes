SpringMVC
一、控制器
1.有@Controller， @RestController注解的类叫做控制器， 项目中控制器有很多
 一般相关的业务是一个控制器， 比如订单的处理工作， 订单的控制OrderController.

2.@Controller， @RestController区别
   @RestController包含了@Controller的功能，同时加入了@ResponseBody的注解。
   表示当前控制器类中的所有方法，都会默认加入@ResponseBody的功能。 方法的返回值是数据可以通过
   HttpServletResponse输出给浏览器，请求方法。

   @Controller注解有@Component的功能， 控制器类对象是Spring容器管理的。

3.定制控制器方法
  1.方法上面的@RequestMapping(value="请求的uri地址") ：表示这个uri的请求由当前方法处理
    @GetMapping, @PostMapping, @PutMapping , @DeleteMapping
  2.控制器方法的形参，接收请求的参数，多种方法接收参数
  3.控制器方法的返回值，表示应答结果，给请求的结果（数据，视图）

SpringMVC处理请求 围绕着控制器的方法
1.public方法。
2.方法名称自定义
3.方法的形参表示接收的请求参数
   1）逐一按名称对应接收参数。String name, Integer age
   2）使用自定义对象，接收多个参数 User(name,age, sex)
   3）接收json， 使用@RequestBody注解，加上自定义对象
   4）使用Reader，InputStream作为参数，读取post请求体的内容
   5）使用HttpServletRequest作为参数，调用getParameter()方法接收参数
   6）数组参数Integer [] id
   7) @RequestParam
   8) @RequestHeader

4.控制器方法的返回值，表示给请求方的应答。响应内容
  1）String，表示视图页面（xxx.html), 没有使用@ResponseBody。
  2）ModelAndView:是数据和视图结合体， 包含了数据和视图
  3）自定义对象： 默认将自定义对象转为json格式，输出给浏览器
  4）ResponseEntity：包含数据和自定义的HttpStatus Code
  5）Map：默认将key和value转为json字符串。

======================================================================================
测试请求，使用浏览器， PostMan， ApiFox , IDEA中的HttpClient
======================================================================================

SpringMVC自动配置：
DispatcherServletAutoConfiguration:配置中央调度器，设置他的名称， load-on-start:-1, 默认的url-pattern: /
WebMvcConfigurationSupport:配置SpringMVC的支持对象， HandlerMapping, HandlerAdapter, ViewResolver, HandlerExceptionResolver
                           HttpMessageConverters .. 静态资源的对象
ServletWebServerFactoryAutoConfiguration:配置Tomcat服务器， Jetty服务器

核心对象：
DispatcherServlet：Servlet， 使用doService()接收并处理请求。 前端控制器

处理业务：
后端控制器， 开发人员自定义的Controller， 使用注解@Controller， @RestController

帮助处理请求的对象：
HandlerMapping: 根据请求的uri地址，找到处理此请求的Controller对象（Spring容器中的）
HandlerAdapter：使用适配器模式，调用执行具体的控制器方法。
ViewResolver：处理视图，创建视图对象View。
HandlerExceptionResolver：异常处理器，处理请求中的异常。

注解
1）控制器@Controller, @RestController
2) 接收请求 @RequestMapping, @GetMapping, @PostMapping, @PutMapping ,@DeleteMapping
3) 接收参数 @RequestParam ,@RequestHeader, @RequestBody
4) 校验参数 @Validated , JSR 303注解
5) 返回值表示数据的@ResponseBody
6) 支持RESTful的路径变量 @PathVariable




