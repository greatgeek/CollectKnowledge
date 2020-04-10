#### 1. SpringMVC的基本概念

#### 1.1 关于三层架构与MVC

#### 1.1.1 三层架构

开发架构一般基于两种形式，一种是C/S架构（Client/Server） ，另一种是B/S架构（Browser/Server）。在Java EE开发中，几乎全都是基于B/S架构开发的，系统标准的三层架构包括：表现层、业务层、持久层。

**表现层：SpringMVC**

也就是我们常说的web层。它负责接收客户端请求，向客户端响应结果，通常客户端使用http协议请求web层，web需要接收http请求，完成http响应。

表现层包括展示层和控制层：控制层负责接收请求，展示层负责结果的展示。

表现层依赖业务层，接收到客户端请求一般会调用业务层进行业务处理，并将处理结果响应给客户端。

表现层的设计一般都使用MVC模型。（MVC是表现层的设计模型，和其他层没有关系）

**业务层：Spring框架**

也就是我们常说的service层。它负责业务逻辑处理，web层依赖业务层，但是业务层不依赖web层。

业务层在业务处理时可能会依赖持久层，如果要对数据持久化需要保证事务的一致性。

**持久层：MyBatis**

也就是我们常说的DAO层。负责数据持久化，包括数据层即数据库和数据访问层，数据库是对数据进行持久化的载体，数据访问层是业务层和持久层交互的接口，业务层需要通过数据访问层将数据持久化到数据库中。通俗地讲，持久层就是与数据库交互，对数据库表进行增删改查。

#### 1.1.2 MVC 模型

Model/View/Controller

#### 1.2 SpringMVC 和 Struts2的优略分析

**共同点：**

它们都是表现层框架，都是基于MVC模型编写的。

它们的底层都离不开原始的ServletAPI。

它们处理请求的机制都是一个核心控制器。

**区别：**

Spring MVC的入口是Servlet，而Struts2 是 Filter

Spring MVC 是基于方法设计的，而Struts2 是基于类，Struts2 每次执行都会创建一个动作类。所以Spring MVC会稍微比 Struts2快些。

Spring MVC 使用更加简洁，同时还支持JSR303，处理ajax的请求更方便。

（JSR 303 是一套JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注解加在我们JavaBean的属性上面，就可以在需要校验的时候进行校验了。）

Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些，但执行效率并没有比 JSTL 提
升，尤其是 struts2 的表单标签，远没有 html 执行效率高。

#### 2. Spring MVC中涉及的组件

Spring MVC是以组件的方式运行的。

#### 2.1 DispatcherServlet: 前端控制器

用户请求到达前端控制器，它就相当于MVC模式中的C，dispatcherServlet 是整个流程控制的中心，由它调用其他组件处理用户的请求，dispatcherServlet的存在降低了组件之间的耦合性。

Spring MVC, as many other web frameworks, is designed around the front controller pattern where a central ``` Servlet```, the ``` DispatcherServlet```，provides a shared algorithm for request processing, while actual work is performed by configurable delegate components. This model is flexible and supports diverse workflows.

The ``` DispatcherServlet```,as any ``` Servlet```, **needs to be declared and mapped according to the Servlet specification by using Java configuration or in ``` web.xml```.In turn**, the ``` DispatcherServlet``` uses Spring configuration to discover the delegate components it needs for request mapping, view resolution, exception handling, and more.

#### 2.2 HandlerMapping: 处理器映射器

HandlerMapping 负责根据用户请求找到 Handler 即处理器，Spring MVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

#### 2.3 Handler: 处理器

它就是我们开发中要编写的具体业务控制器。由 ``` DispatcherServlet``` 把用户请求转发到 Handler。由Handler对具体的用户请求进行处理。

#### 2.4 HandlerAdapter: 处理器适配器

通过HandlerAdapter 对处理器进行执行，这是适配器模式模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

#### 2.5 View Resolver: 视图解析器

View Resolver 负责将处理结果生成 View视图，View Resolver 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成View 视图对象，最后对View 进行渲染将处理结果通过页面展示给用户。

#### 2.6 View：视图

Spring MVC 框架提供了很多的View 视图类型的支持，包括：jstlView、freemarkerView、pdfView等。我们最常用的视图就是jsp。

一般情况下需要通过页面标签或页面模板技术将模型数据通过页面展示给用户，需要由程序员根据业务需要开发具体的页面。

#### 2.7 Special Bean Types

The ``` DispatcherServlet``` delegates to special beans to process requests and render the appropriate responses. By "special beans" we mean Spring-managed ``` Object``` instances that implement framework contracts. Those usually come with built-in contracts, but you can customize their properties and extend or replace them.

The following table lists the special beans detected by the ```DispatcherServlet ```.

| **Bean type**  | **Explanation**                                              |
| :------------- | :----------------------------------------------------------- |
| HandlerMapping | Map a request to a handler along with a list of interceptors for pre-and post-processing. The mapping is based on some criteria, the details of which vary by ``` HandlerMapping``` implementation.<br />The two main ``` HandlerMapping``` implementations are ``` RequestMappingHandlerMapping```(which supports ``` @RequestMapping``` annotated methods) and ``` SimpleUrlHandlerMapping```(which maintains explicit registrations of URI path patterns to handlers). |
| HandlerAdapter | Help the ```DispatcherServlet``` to invoke a handler mapped to a request , regardless of how the handler is actually invoked. For example, invoking an annotated controller requires resolving annotations. The main purpose of a ``` HandlerAdapter``` is to shield the ``` DispatcherServlet``` from such details. |
| ViewResolver   | Resolve logical ```String```-based view names returned from a handler to an actual ``` View``` with which to render to the response. |

在SpringMVC的各个组件中，处理器映射器、处理器适配器、视图解析器称为SpringMVC的三大组件。

使用``` <mvc:annotation-driven/>``` 自动加载RequestMappingHandlerMapping（处理映射器）和RequestMappingHandlerAdapter（处理适配器），可用在SpringMVC.xml 配置文件中使用``` <mvc:annotation-driven/>```替代注解处理器和适配器的配置。

### 3. 请求参数的绑定

SpringMVC 绑定请求参数的过程是通过把表单提交请求参数，作为控制器中方法参数进行绑定的。