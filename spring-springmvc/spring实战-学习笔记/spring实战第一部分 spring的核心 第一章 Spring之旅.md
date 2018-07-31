## 1.1 简化java开发
为了降低Java开发的复杂性，Spring采取了以下4种关键策略：
- 基于POJO的轻量级和最小侵入性编程；
- 通过依赖注入和面向接口实现松耦合；
- 基于切面和惯例进行声明式编程；
- 通过切面和模板减少样板式代码。

### 1.1.2 依赖注入 
耦合具有两面性（two-headed beast）。
- 一方面，紧密耦合的代码难以
测试、难以复用、难以理解，
- 另一方面，一
定程度的耦合又是必须的——完全没有耦合的代码什么也做不了

通过依赖注入（==管理对象之间的依赖==）会将所依赖的关系自动交给目标对象，而不是让对象自己去获取依赖

通过构造函数，进行接口的注入，实际生成对象时，传入实现了接口的具体类，==实现类与类的松耦合==

---     
创建应用组件之间协作的行为通常称为装配（wiring）。Spring有多
种装配bean的方式，采用XML是很常见的一种装配方式。  
如果XML配置不符合你的喜好的话，Spring还支持使用Java来描述配置

#### Spring通过应用上下文（==Application Context==）装载bean的定义并把它们组装起来
Spring应用上下文全权负责对象的创建和组装。Spring
自带了多种应用上下文的实现，它们之间主要的区别仅仅在于如何加载配置。
- 使用XML文件进行配置 ClassPathXmlApplicationContext

### 1.1.3 应用切面  
DI能够让相互协作的软件组件保持松散耦合，而面向切面编程（aspect-oriented programming，AOP）允许你把遍布应用各处的功能分离出来形成可重用的组件。

诸如日志、事务管理和安全这样的系统服务经常融入到自身具有核心业务逻辑的组件中去，这些系统服务通常被称为==横切关注点==，因为它们会跨越系统的多个组件。

### 1.1.4　使用模板消除样板式代码 p42
Spring旨在通过模板封装来消除样板式代码。Spring的JdbcTemplate使
得执行数据库操作时，避免传统的JDBC样板代码成为了可能。

## 1.2　容纳你的Bean   p45
在基于Spring的应用中，你的应用对象生存于Spring容器，    
Spring容器负责创建对象，装配它们，配置它们并
管理它们的整个生命周期，从生存到死亡（在这里，可能就是new到finalize()）。   
==容器是Spring框架的核心==

Spring容器并不是只有一个。Spring自带了多个容器实现，可以归为两种不同的类型  
- bean工厂（由org.springframework. beans.
factory.eanFactory接口定义）是最简单的容器，提供基本的DI支持
- ==应用上下文==
（由org.springframework.context.ApplicationContext
接口定义）基于BeanFactory构建，并提供应用框架级别的服务，例如
==从属性文件解析文本信息==以及==发布应用事件给感兴趣的事件监听者==。    
### 1.2.1　使用应用上下文
Spring自带了多种类型的应用上下文
- AnnotationConfigApplicationContext：从一个或多个基于Java的配置类中加载Spring应用上下文。
- AnnotationConfigWebApplicationContext：从一个或
多个基于Java的配置类中加载Spring Web应用上下文。
- ==ClassPathXmlApplicationContext==：从类路径下的一个或
多个XML配置文件中加载上下文定义，把应用上下文的定义文件
作为类资源。
- ==FileSystemXmlapplicationcontext==：从文件系统下的一
个或多个XML配置文件中加载上下文定义。
- XmlWebApplicationContext：从Web应用下的一个或多个
XML配置文件中加载上下文定义。

==FileSystemXmlApplicationContext==在指定的文件系统路
径下查找knight.xml文件；        
==ClassPathXmlApplicationContext==
是在所有的类路径（包含JAR文件）下查找 knight.xml文件。

应用上下文准备就绪之后，我们就可以调用上下文的==getBean==()方法从Spring容器中获取bean。
### 1.2.2　bean的生命周期 p47
在传统的Java应用中，bean的生命周期很简单。使用Java关键字new进
行bean实例化，然后该bean就可以使用了。一旦该bean不再被使用，
则由Java自动进行垃圾回收。  
相比之下，Spring容器中的bean的生命周期就显得相对复杂多了。　    
bean在Spring容器中从创建到销毁经历了
若干阶段，每一阶段都可以针对Spring如何管理bean进行个性化定制    
1. Spring对bean进行实例化； 
2. Spring将值和bean的引用注入到bean对应的属性中；
3. 如果bean实现了BeanNameAware接口，Spring将bean的ID传递给setBean-Name()方法；
4. 如果bean实现了BeanFactoryAware接口，Spring将调用setBeanFactory()方法，将BeanFactory容器实例传入；
5. 如果bean实现了ApplicationContextAware接口，Spring将调用setApplicationContext()方法，将bean所在的应用上下文的引用传入进来；
6. 如果bean实现了BeanPostProcessor接口，Spring将调用它们
的post-ProcessBeforeInitialization()方法；
7. 如果bean实现了InitializingBean接口，Spring将调用它们的
after-PropertiesSet()方法。类似地，如果bean使用initmethod
声明了初始化方法，该方法也会被调用；
8. 如果bean实现了BeanPostProcessor接口，Spring将调用它们
的post-ProcessAfterInitialization()方法；    
9. 此时，bean已经准备就绪，可以被应用程序使用了，它们将一直
驻留在应用上下文中，直到该应用上下文被销毁；
10. 如果bean实现了DisposableBean接口，Spring将调用它的
destroy()接口方法。同样，如果bean使用destroy-method声明了销毁方法，该方法也会被调用。

现在你已经了解了如何创建和加载一个Spring容器。但是一个空的容
器并没有太大的价值，在你把东西放进去之前，它里面什么都没有。
为了从Spring的DI中受益，我们必须将应用对象装配进Spring容器中
## 1.3　俯瞰Spring风景线
Spring框架关注于通过DI、AOP和消除样板式代码来简化企业级Java开发  
在Spring框架之外还存在一个构建在核心框架之上的庞大生态圈，它
将Spring扩展到不同的领域，例如Web服务、REST、移动开发以及NoSQL。
### 1.3.1　Spring模块
> Spring核心容器    

容器是Spring框架最核心的部分，它管理着Spring应用中bean的创建、
配置和管理。在该模块中，包括了Spring bean工厂，它为Spring提供
了DI的功能。基于bean工厂的多种Spring应用上下文的实现，每一种都提供了配置Spring的不同方式。       
除了bean工厂和应用上下文，该模块也提供了许多企业服务，例如Email
、JNDI访问、EJB集成和调度。     
所有的Spring模块都构建于核心容器之上
> Spring的AOP模块       

在AOP模块中，Spring对面向切面编程提供了丰富的支持。这个模块
是Spring应用系统中开发切面的基础。

与DI一样，AOP可以帮助应用对象解耦。借助于AOP，可以将遍布系统的关注点（例如事务和安全）从它们所应用的对象中解耦出来。
>数据访问与集成 

使用JDBC编写代码通常会导致大量的样板式代码，例如获得数据库
连接、创建语句、处理结果集到最后关闭数据库连接。    
Spring的JDBC
和DAO（Data Access Object）模块抽象了这些样板式代码，使数据库代码变得简单明了，还可以避免因为关闭数据库资源失败而引发的问题    
对于那些更喜欢ORM（Object-Relational Mapping）工具而不愿意直接
使用JDBC的开发者，Spring提供了ORM模块。    
Spring的ORM模块建立
在对DAO的支持之上，并为多个ORM框架提供了一种构建DAO的简
便方式。    
Spring没有尝试去创建自己的ORM解决方案，而是对许多流行的ORM框架进行了集成  
Spring的事务管理支持所有的
ORM框架以及JDBC。   
本模块同样包含了在JMS（Java Message Service）之上构建的Spring
抽象层，它会使用消息以异步的方式与其他应用集成      
从Spring 3.0开
始，本模块还包含对象到XML映射的特性，它最初是Spring Web
Service项目的一部分。   
除此之外，本模块会使用Spring AOP模块为Spring应用中的对象提供
事务管理服务。
>Web与远程调用  

spring的Web和远程
调用模块自带了一个强大的MVC框架，有助于在Web层提升应用的松
耦合水平
>测试

鉴于开发者自测的重要性，Spring提供了测试模块以致力于Spring应用的测试。  

通过该模块，你会发现Spring为使用JNDI、Servlet和Portlet编写单元
测试提供了一系列的mock对象实现。    
对于集成测试，该模块为加载
Spring应用上下文中的bean集合以及与Spring上下文中的bean进行交互提供了支持
### 1.3.2　Spring Portfolio
整个Spring
Portfolio包括多个构建于核心Spring框架之上的框架和类库。概括地讲，整个Spring Portfolio几乎为每一个领域的Java开发都提供了Spring
编程模型。
>Spring Web Flow    

Spring Web Flow建立于Spring MVC框架之上，它为基于流程的会话式
Web应用（可以想一下购物车或者向导功能）提供了支持
> Spring Web Service    

> Spring Security

安全对于许多应用都是一个非常关键的切面。利用Spring AOP，
Spring Security为Spring应用提供了声明式的安全机制。
>Spring Integration 

许多企业级应用都需要与其他应用进行交互。Spring Integration提供了
多种通用应用集成模式的Spring声明式风格实现。
>Spring Batch   

当我们需要对数据进行大量操作时，没有任何技术可以比批处理更胜
任这种场景。如果需要开发一个批处理应用，你可以通过Spring
Batch，使用Spring强大的面向POJO的编程模型。    
> Spring Data   

Spring Data使得在Spring中使用任何数据库都变得非常容易  
Spring Data都为持久化提供了一种简单的编程模
型。这包括为多种数据库类型提供了一种自动化的Repository机制，
它负责为你创建Repository的实现。
> Spring Social

>Spring Mobile  

移动应用是另一个引人瞩目的软件开发领域。智能手机和平板设备已
成为许多用户首选的客户端。Spring Mobile是Spring MVC新的扩展模
块，用于支持移动Web应用开发。
> Spring for Android
---
### Spring Boot
spring简化Java开发，spring boot 简化 spring开发  
Spring Boot大量依赖于自动配置技术，它能够消除大部分（在很多场
景中，甚至是全部）Spring配置。  
它还提供了多个Starter项目，不管
你使用Maven还是Gradle，这都能减少Spring工程构建文件的大小。
## 1.4　Spring的新功能 p57
### 1.4.1　Spring 3.1新特性
### 1.4.2　Spring 3.2新特性
### 1.4.3　Spring 4.0新特性
## 1.5　小结 p61
Spring致力
于简化企业级Java开发，促进代码的松散耦合。成功的关键在于依赖注入和AOP。

DI是组装应用对象的一种方式，对象无需知道依赖来自何处或者依赖的实现方式  

依赖对象通常会通过接口了解所注入的对象，这样的话就能确保低耦合。