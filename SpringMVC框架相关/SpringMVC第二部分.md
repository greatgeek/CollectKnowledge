#### Spring MVC 第二部分

#### 1. 响应数据和结果视图

#### 1.1 返回值分类

**Return Values**

The next table describes the supported controller method return values. Reactive types are supported for all return values.

| Controller method return value        | Description                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ```@ResponseBody```                   | The return value is converted through ``` HttpMessageConverter``` implements and written to the response. |
| ```HttpEntity<B>,ResponseEntity<B>``` | The return value that specifies the full response (including HTTP headers and body) is to be converted through `HttpMessageConverter` implementations and written to the response. |
| ```HttpHeaders```                     | For returning a response with headers and no body.           |
| ``` String```                         | A view name to be resolved with `ViewResolver` implementations and used together with the implicit model — determined through command objects and `@ModelAttribute` methods. The handler method can also programmatically enrich the model by declaring a `Model` argument (see [Explicit Registrations](https://docs.spring.io/spring/docs/5.2.5.RELEASE/spring-framework-reference/web.html#mvc-ann-requestmapping-registration)). |
| ``` void```                           | A method with a `void` return type (or `null` return value) is considered to have fully handled the response if it also has a `ServletResponse`, an `OutputStream` argument, or an `@ResponseStatus` annotation. The same is also true if the controller has made a positive `ETag` or `lastModified` timestamp check (see [Controllers](https://docs.spring.io/spring/docs/5.2.5.RELEASE/spring-framework-reference/web.html#mvc-caching-etag-lastmodified) for details). |



#### 1.1.1 字符串

```@Controller ```注解修饰的类的方法返回字符串可以指定逻辑视图名，通过视图解析器解析为物理视图地址。例如常用的返回xxx/success.jsp页面。

#### 1.1.2 void

可以在方法参数中定义```HttpServletRequest```和``` HttpServletResponse```，使用``` request```和``` response```指定响应结果：

1、使用request转向页面

```java
request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request,response);
```

2、也可以通过response页面重定向

```java
response.sendRedirect("testRetrunString");
```

3、也可以通过response指定响应结果，例如响应json数据

```java
response.setCharacterEncoding("utf-8");
response.setContentType("application/json;charset=utf-8");
response.getWriter().write("json 串");
```

#### 1.1.3 ModelAndView

ModelAndView是SpringMVC为我们提供的一个对象，该对象也可以用作控制器方法的返回值。

#### 1.2 转发和重定向

#### 1.2.1 forward转发

Controller 方法提供了String类型的返回值之后，默认就是请求转发。

```java
return "forward:/WEB-INF/pages/success.jsp";
```

需要注意的是，如果用了```forward:```，则路径必须写成实际视图url，不能写逻辑视图。

它相当于``` request.getRequestDispatcher("url").forward(request,response)```。使用请求转发，即可以转发到jsp，也可以转发到其他的控制器方法。

**Forwarding**

You can also use a special ```forward:```prefix for view names that are ultimately resolved by ```UrlBasedViewResolver``` and subclasses. This creates an ```InternalResourceView```, which does a ```RequestDispatcher.forward()```. Therefore, this prefix is not useful with `InternalResourceViewResolver` and `InternalResourceView` (for JSPs), but it can be helpful if you use another view technology but still want to force a forward of a resource to be handled by the Servlet/JSP engine. Note that you may also chain multiple view resolvers, instead.

#### 1.2.2 Redirect 重定向

Controler 方法提供了一个String类型返回值之后，它需要在返回值里使用：```redirect: ```。

以下是请求到其他控制方法。

```java
return "redirect:testReturnModelAndView";
```

它相当于```response.sendRedirect(url)```。需要注意的是，如果是重定向到```jsp```页面，则```jsp```页面不能写在```WEB-INF```目录中，否则无法找到。

**Redirecting**

The special ```redirect:``` prefix in a view name lets you perform a redirect. The `UrlBasedViewResolver` (and its subclasses) recognize this as an instruction that a redirect is needed. The rest of the view name is the redirect URL.

The net effect is the same as if the controller had returned a `RedirectView`, but now the controller itself can operate in terms of logical view names. A logical view name (such as `redirect:/myapp/some/resource`) redirects relative to the current Servlet context, while a name such as `redirect:https://myhost.com/some/arbitrary/path` redirects to an absolute URL.

Note that, if a controller method is annotated with the `@ResponseStatus`, the annotation value takes precedence over the response status set by `RedirectView`.

---

异步请求：发送方发出数据后，等接收方发回响应以后才发下一个数据包的通讯方式。

同步请求：发送方发出数据后，不等接收方发回响应，接着发送下一个数据包的通讯方式。

---

#### 1.3 @ResponseBody 响应 json 数据

该注解用于将 Controller 的方法返回的对象，通过 HttpMessageConverter 接口转换为指定格式的数据。如: json, xml 等，通过 Response 响应给客户端。

#### 2. SpringMVC 实现文件上传

#### 2.1 传统方式 Apache Commons ```FileUpload```

The Commons **FileUpload** package makes it easy to add robust, high-performance, file upload capability to your servlets and web applications.

FileUpload depends on Commons IO, so make sure you have the version mentioned on the [dependencies page](http://commons.apache.org/proper/commons-fileupload/dependencies.html) in          your classpath before continuing.      

参考来源：[Commons FileUpload](http://commons.apache.org/proper/commons-fileupload/index.html)

#### 2.2 Spring MVC 方式

#### 3. SpringMVC 中的异常处理

**Exceptions**

If an exception occurs during request mapping or is thrown from a request handler(such as ```@Controller```),the ``` DispatcherServlet``` delegates to a chain of ``` HandlerExceptionResolver``` beans to resolve the exception and provide alternative handling, which is typically an error response.

The following table lists the available `HandlerExceptionResolver` implementations:

| HandlerExceptionResolver          | Description                                                  |
| --------------------------------- | ------------------------------------------------------------ |
| SimpleMappingExceptionResolver    | A mapping between exception class names and error view names. Useful for rendering error pages in a browser application. |
| DefaultHandlerExceptionResolver   | Resolves exceptions raised by Spring MVC and maps them to HTTP status codes. |
| ResponseStatusExceptionResolver   | Resolves exceptions with the ``` @ResponseStatus``` annotation and maps them to HTTP status codes based on the value in the annotation. |
| ExceptionHandlerExceptionResolver | Resolves exceptions by invoking an ``` @ExceptionHandler``` method in a ``` @Controller``` or a ``` @ControllerAdvice``` class. |

##### Chain of Resolvers

You can form an exception resolver chain by declaring multiple `HandlerExceptionResolver` beans in your Spring configuration and setting their `order` properties as needed. The higher the order property, the later the exception resolver is positioned.

The contract of `HandlerExceptionResolver` specifies that it can return:

- a `ModelAndView` that points to an error view.
- An empty `ModelAndView` if the exception was handled within the resolver.
- `null` if the exception remains unresolved, for subsequent resolvers to try, and, if the exception remains at the end, it is allowed to bubble up to the Servlet container.

The [MVC Config](https://docs.spring.io/spring/docs/5.2.5.RELEASE/spring-framework-reference/web.html#mvc-config) automatically declares built-in resolvers for default Spring MVC exceptions, for `@ResponseStatus` annotated exceptions, and for support of `@ExceptionHandler` methods. You can customize that list or replace it.

#### 4. Spring MVC 中的拦截器

Spring MVC 的处理器拦截器类似于 Servlet 开发中的过滤器 Filter，用于对处理器进行预处理和后处理。

用户可以自己定义一些拦截器来实现特定的功能。

拦截器链（Interceptor Chain），拦截器链就是将拦截器按一定的顺序联结成一条链。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。

**过滤器与拦截器的区别**

过滤器是servlet 规范中的一部分，任何 java web 工程都可以使用。

拦截器是 Spring MVC 框架自己的，只有使用了 SpringMVC 框架的工程才能用。

过滤器在 url-pattern 中配置了 /* 之后，可以对所有要访问的资源拦截。

拦截器它是只会拦截访问控制器的方法，如果访问的是 jsp, html, css, image或者js 是不会进行拦截的。

它也是AOP思想的具体应用。

我们要想自定义拦截器，要求必须实现： HandlerInterceptor 接口。

**Interception**

All ``` HandlerMapping``` implementations support handler interceptors that are useful when you want to apply specific functionality to certain requests-- for example, checking for a principal. Interceptors must implement ``` HandlerInterceptro``` from the `org.springframework.web.servlet` package with three methods that should provide enough flexibility to do all kinds of pre-processing and post-processing:

- `preHandle(..)`: Before the actual handler is executed
- `postHandle(..)`: After the handler is executed
- `afterCompletion(..)`: After the complete request has finished

The `preHandle(..)` method returns a boolean value. You can use this method to break or continue the processing of the execution chain. When this method returns `true`, the handler execution chain continues. When it returns false, the `DispatcherServlet` assumes the interceptor itself has taken care of requests (and, for example, rendered an appropriate view) and does not continue executing the other interceptors and the actual handler in the execution chain.

See [Interceptors](https://docs.spring.io/spring/docs/5.2.5.RELEASE/spring-framework-reference/web.html#mvc-config-interceptors) in the section on MVC configuration for examples of how to configure interceptors. You can also register them directly by using setters on individual `HandlerMapping` implementations.

Note that `postHandle` is less useful with `@ResponseBody` and `ResponseEntity` methods for which the response is written and committed within the `HandlerAdapter` and before `postHandle`. That means it is too late to make any changes to the response, such as adding an extra header. For such scenarios, you can implement `ResponseBodyAdvice` and either declare it as an [Controller Advice](https://docs.spring.io/spring/docs/5.2.5.RELEASE/spring-framework-reference/web.html#mvc-ann-controller-advice) bean or configure it directly on `RequestMappingHandlerAdapter`.