Servlet是JavaWeb技术的核心基础，本章重点
- Servlet容器是如何工作的
- JavaWeb是如何基于 Servlet工作的
- Web工程在Servlet容器中是如何启动的
- Servlet容器如何解析 在web.xml中定义的Servlet
- 用户请求是如何被分配给指定的Servlet的
- Servlet容器如何管理Servlet生命周期
- Servlet的API层次结构

### 9.1 从Servlet说起
>接口是连接 Servlet和 Servlet容器的关键， 在Tomcat的容器等级中，Context容器 直接管理 Servlet在容器中的包装类 Wrapper，所以Context容器将如何运行将直接影响Servlet的工作方式。Tomcat容器模型如下    p244

>Tomcat的容器分为4个等级， 真正管理Servlet的容器是 Context容器，一个Context对应一个Web工程，在Tomcat的配置文件中可以看出
    
    <Context path="/projectOne" docBase="D:\projects\projectOne" reloadable="trun" />
>下面介绍 Tomcat解析 Context容器的过程，包括如何 构件Servlet
#### 9.1.1 Servlet容器的启动过程
    一个Web应用对应一个Context容器，也就是 Servlet运行时的 Servlet容器。  
    添加一个 Web应用时将会创建一个 StandardContext容器，并且给这个Context容器设置必要的参数，
    url和path分别代表 这个应用在Tomcat中的 访问路径和这个应用的实际物理路径，这两个参数与Tomcat配置中的两个参数是一致的，
    其中最重要的一个配置是 ContextConfig， 这个类将负责整个Web应用配置的解析工作，最后将这个Context容器加到 父容器 Host中

>接下来会调用Tomcat的start方法启动 Tomcat。Tomcat的启动逻辑是基于观察者模式设计的，多有的容器都会继承 **Lifecycle接口**， 它管理着容器的整个生命周期， 所有容器的修改和状态的改变 都会由它去通知已经注册的观察者（Listener）

### Web应用对应的StandardContext容器的启动过程

#### 9.1.2 Web应用的初始化工作      p247

>为什么要将Servlet 包装成 StandardWrapper 而不直接 包装成 Servlet对象？  这里的StandardWrapper是 Tomcat容器中的一部分，它具有容器的特征， 而Servlet作为一个Web开发标准，不应该强耦合在Tomcat中

>除了将Servlet 包装成 StandardWrapper 并作为子容器添加到 Context中， 其他web.xml属性都被解析到Context中，**所以Context容器才是真正运行 Servlet的Servler容器**，容器的配置属性 由应用的 web.xml指定
### 9.2 创建 Servlet实例
Servlet解析之后被包装成 StandardWrapper添加在Context容器中，然后被实例化
#### 9.2.1 创建 Servlet对象


#### 9.2.2  初始化 Servlet      p251
事实上Servlet从被 web.xml 解析到完成初始化非常复杂

### 9.3  Servlet 体系结构
>Servlet规范是基于这几个类运转的，分别是
- ServletConfig
- ServletRequest
- ServletResponse


### 9.4 Servlet 如何工作
>我们已经知道了 Servlet是如何被加载、如何被初始化，以及Servlet的体系结构，现在是问题是 它是如何被调用的

>用户从浏览器向服务器 发起的一个请求通常会包含如下信息
    
    http://hostname:port/contextpath/servletpath
>其中 hostname和port用来与服务器建立 TCP连接，后面的URL用来选择服务器的子容器处理用户的请求 ， Tomcat7中 有专门的类负责 映射 HTTP请求和对应的Servlet容器

    org.apache.tomcat.util.http.mapper
    
>MVC框架的基本原理是将所有的请求 映射到某一个Servlet，然后去实现 service方法，这个方法就是 MVC框架的入口

>当 Servlet从Servlet容器中移除时，也就表明 该Servlet的生命周期结束了，这时 Servlet的 destroy方法将被调用
### 9.5 Servlet中的 Listener
>在整个Tomcat服务器中，Listener使用的非常广泛，它是基于观察者模式设计的，Listener的设计 为开发 Servlet应用程序 提供了便捷，能够从一个纵向维度控制程序和数据，目前Servlet提供两类事件的观察者接口，共6种，分别是

    EventLosteners类型
        ServletContextAttributeListener
        ServletRequestAttributeListener
        ServletRequestListener
        HttpSessionAttributeListener
    LifecycleListeners类型
        ServletContextListener
        HttpSessionListener
        
### 9.6 Filter 如何工作         p260

>Filter类的核心是传递的 FilterChain 对象


### 9.7 Servlet中的 url-pattren

### 9.8 总结
- Servlet容器的启动
- Servlet的初始化
- Servlet的体系结构

    