## 21.1　Spring Boot简介
它提供了四个主要的特性，能够改变开发Spring应用程序的方式：
- 使用Spring Boot Starter添加项目依赖
- 自动化的 bean 配置
- Groovy 与 Spring Boot CLI
- Spring Boot Actuator
---
> 使用Spring Boot Starter添加项目依赖

它将常用的依赖分组进行了整合，将其合并到一个依赖中，这样就可以一次性添加到项目的Maven或Gradle构建中；
> 自动配置：    

Spring Boot的自动配置特性利用了Spring 4对条件化配
置的支持，合理地推测应用所需的bean并自动化配置它们
> 命令行接口（Command-line interface，CLI）：  

Spring Boot的CLI
发挥了Groovy编程语言的优势，并结合自动配置进一步简化
Spring应用的开发；
> Actuator：    

它为Spring Boot应用添加了一定的管理特性。
### 21.1.1　添加Starter依赖  p649
Spring Boot Starter依赖将所需的常见依赖按组聚集在一起，形成单条依赖  
Starter的工作方式也
没有什么神秘之处。它使用了Maven和Gradle的==依赖传递方案==，Starter在自己的pom.xml文件中声明了多个依赖。   
当我们将某一个Starter依赖
添加到Maven或Gradle构建中的时候，Starter的依赖将会自动地传递性
解析。  
这些依赖本身可能也会有其他的依赖。一个Starter可能会传递性地引入几十个依赖。
### 21.1.2　自动配置
Spring Boot的Starter减少了构建中依赖列表的长度，而Spring Boot的
自动配置功能则削减了Spring配置的数量

它在实现时，会考虑应用
中的其他因素并推断你所需要的Spring配置。    

Spring Boot Starter也会触发自动配置。   

例如，在Spring Boot应用中，
如果我们想要使用Spring MVC的话，所需要做的仅仅是将Web Starter
作为依赖放到构建之中。  
将Web Starter作为依赖放到构建中以后，它会自动添加Spring MVC依赖。如果Spring Boot的Web自动配置探测到
Spring MVC位于类路径下，它将会自动配置支持Spring MVC的多个
bean，包括视图解析器、资源处理器以及消息转换器（等等）。    
我们
接下来需要做的就是编写处理请求的控制器。
### 21.1.3　Spring Boot CLI
Spring Boot CLI充分利用了Spring Boot Starter和自动配置的魔力，并
添加了一些Groovy的功能。    
它简化了Spring的开发流程，通过CLI，我
们能够运行一个或多个Groovy脚本，并查看它是如何运行的。  
在应用
的运行过程中，CLI能够自动导入Spring类型并解析依赖    
在21.3小节中，我们将会看到如何使用Groovy和CLI构建更加完整的应用
### 21.1.4　Actuator
Spring Boot Actuator为Spring Boot项目带来了很多有用的特性，包括：
- 管理端点；
- 合理的异常处理以及默认的“/error”映射端点；
- 获取应用信息的“/info”端点；
- 当启用Spring Security时，会有一个审计事件框架。
## 21.2　使用Spring Boot构建应用 p656
可以自由选择使用Maven还是Gradle来构建应用程序    
### 21.2.1　处理请求 