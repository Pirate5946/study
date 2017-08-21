本章内容：
- 映射请求到Spring控制器
- 透明地绑定表单参数
- 校验表单提交

### 5.1.1　跟踪Spring MVC的请求
#### 请求是如何从客户端发起，经过Spring MVC中的组件，最终再返回到客户端的。

在请求离开浏览器时，会带有用户所请求内容的信息，至少会包含
请求的URL。但是还可能带有其他的信息，例如用户提交的表单信息

1、请求旅程的第一站是Spring的==DispatcherServlet==    

==前端控制器==是常用的Web应用程序模
式，在这里==一个单实例的Servlet将请求委托给应用程序的其他组件==来
执行实际的处理。    
在Spring MVC中，DispatcherServlet就是前端控制器。

2、DispatcherServlet以会查
询一个或多个==处理器映射（handler mapping）== 来确定应该将请求发送给哪个控制器。        
处理器映射会根据请求所携带的URL信息来进行决策。

3、一旦选择了合适的控制器，DispatcherServlet会将请求发送给
选中的控制器。==到了控制器，请求会卸下用户提交的信息并耐心等待控制器处理这些信息。==
实际上，设计良好的控制器
本身只处理很少甚至不处理工作，而是将业务逻辑委托给一个或多个
服务对象进行处理。

==控制器在完成逻辑处理后，通常会产生一些信息==，这些信息需要返回
给用户并在浏览器上显示。这些信息被称为==模型（model）==。   
不过仅仅给用户返回原始的信息是不够的——这些信息需要==以用户友好的方式进行格式化==，一般会是HTML。所以，信息需要发送给一个==视图（view）==，通常会是JSP

4、控制器所做的最后一件事就是==将模型数据打包==，并且标示出用于渲染
输出的视图名。  
它接下来会==将请求连同模型和视图名发送回DispatcherServlet==

控制器不会与特定的视图相耦合，传递给
DispatcherServlet的视图名并不直接表示某个特定的JSP。   
实际上，它甚至并不能确定视图就是JSP。相反，它仅仅传递了一个逻辑名称，这个名字将会用来查找产生结果的真正视图。

5、DispatcherServlet将会使用==视图解析器（view resolver）==
来将逻辑视图名匹配为一个特定的视图实现，它可能是也可能不是
JSP。在这里它交付模型数据。请求的任务就完成了。    

6、视图将使用模型数据渲染输出，这个输出会通过响应对象传递给客户端（不会像听上去那样硬编码） 。
### 5.1.2　搭建Spring MVC
#### 配置DispatcherServlet

按照传统的方式，像DispatcherServlet这样的Servlet会配置在
web.xml文件中，这个文件会放到应用的WAR包里面。     
当然，这是配置DispatcherServlet的方法之一。但是，借助于Servlet 3规范和
Spring 3.1的功能增强，这种方式已经不是唯一的方案了
> 程序清单5.1　配置DispatcherServlet p182

在Servlet3.0环境中，==容器会在类路径中查找实现
javax.servlet.ServletContainerInitializer接口的类==，如果能发现的话，就会用它来配置Servlet容器。

Spring提供了这个接口的实现，名
为SpringServletContainerInitializer这个类反过来又会
查找实现WebApplicationInitializer的类并将配置的任务交给它们来完成。  

Spring 3.2引入了一个便利的WebApplicationInitializer基础实现，==AbstractAnnotationConfig DispatcherServlet-
Initializer==

因此当部署到Servlet 3.0容器
中的时候，==容器会自动发现它，并用它来配置Servlet上下文==。

如果==按照这种方式配置DispatcherServlet==，而不是使用web.xml
的话，那唯一问题在于它==只能部署到支持Servlet 3.0的服务器中才能
正常工作，如Tomcat 7或更高版本==。

> 理解DispatcherServlet
和一个Servlet监听器（也就是ContextLoaderListener）的关系。

AbstractAnnotationConfigDispatcherServletInitializer
会同时创建==DispatcherServlet==和
==ContextLoaderListener==。

继承AbstractAnnotationConfig DispatcherServlet-
Initializer需要重写三个方法
- 第一个方法是getServletMappings()，它会将一个或多个路径映
射到DispatcherServlet上。在本例中，它映射的是“/”，这表示
它会是应用的默认Servlet。它会处理进入应用的所有请求。
- GetServlet-ConfigClasses()
方法返回的带有@Configuration注解的类将会==用来定义DispatcherServlet应用上下文中的bean==。
- getRootConfigClasses()方法返回的带有@Configuration注解的类将会==用来配置ContextLoaderListener创建的应用上下文中的bean。==

#### 启用Spring MVC
以前，Spring是使用XML进行配
置的，可以使用<mvc:annotation-driven>启用注解驱动的Spring MVC。

我们所能创建的最简单的Spring MVC配置就是一个带有@EnableWebMvc注解的类：这可以运行起来，它的确能够启用Spring MVC，但还有不少问题要解决：
- 没有配置视图解析器。如果这样的话，Spring默认会使
用BeanNameView-Resolver，这个视图解析器会查找ID与视
图名称匹配的bean，并且查找的bean要实现View接口，它以这样
的方式来解析视图。
- 没有启用组件扫描。这样的结果就是，Spring只能找到显式声明
在配置类中的控制器。
- DispatcherServlet会映射为应用的默认Servlet，所以它会处理所有的请求，包括对静态资源的请求，如
图片和样式表（在大多数情况下，这可能并不是你想要的效
果）。
> 程序清单5.2　最小但可用的Spring MVC配置

### 5.1.3　Spittr应用简介(自己实现简单微博)
在本章中，我们会构建应用的
Web层，创建展现Spittle的控制器以及处理用户注册成为Spitter的表单

## 5.2　编写基本的控制器
在Spring MVC中，控制器只是方法上添加了@RequestMapping注解的类

@RequestMapping注解。它的value属性指定了这个方法所要处理的请求路径，method属性细化了它所处理的HTTP方法。

value属性能够接受一个String类型的数组，表示能够接受到对多个位置的http请求

### 5.2.1　测试控制器 

### 5.2.2　定义类级别的请求处理

### 5.2.3　传递模型数据到视图中

## 5.3　接受请求的输入
Spring MVC允许以多种方式将客户端中的数据传送到控制器的处理器方法中，包括：
- 查询参数（Query Parameter）。
- 表单参数（Form Parameter）。
- 路径变量（Path Variable）。

### 5.3.1　处理查询参数

### 5.3.2　通过路径参数接受输入
如果传递请求中少量的数据，那查询参数和路径变量是很合适的

## 5.4　处理表单

### 5.4.1　编写处理表单的控制器
当InternalResourceViewResolver看到视图格式中
的“==redirect:==”前缀时，它就知道要将其解析为重定向的规则

当InternalResourceViewResolver发现视图格式中以“forward:”作为前缀
时，请求将会前往（forward）指定的URL路径，而不再是重定向。
### 5.4.2　校验表单
Java校验API定义了多个注解，这些注解可以放到属性上，从而限制这些属性的值。所有的注解都位于
==javax.validation.constraints==包中。
> 表5.1　Java校验API所提供的校验解

> 程序清单5.18　Spitter：包含了要提交到Spittle POST请求中的域

## 5.5　小结



































































