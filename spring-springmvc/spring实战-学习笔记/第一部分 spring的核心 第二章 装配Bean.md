本章内容：
- 声明bean
- 构造器注入和Setter方法注入
- 装配bean
- 控制bean的创建和销毁

创建应用对象之间协作关系的行为通常称为装配（wiring），这也是依赖注入（DI）的本质

配置Spring容器最常见的三种方法。
## 2.1　Spring配置的可选方案
Spring容器负责创建应用程序中的bean并通过DI来协调这些对象之间的关系   
它提供了三种主要的装配机制：
- 在XML中进行显式配置。
- 在Java中进行显式配置。
- 隐式的bean发现机制和自动装配。

==Spring的配置风格是可以互相搭配的==，所以你可以选择使用XML装配一些bean，使用Spring基于Java的配置（JavaConfig）来装配另一些bean，而将剩余的bean让Spring去自动发现。

尽可能地使用自动配置的机制。显式配置越少越好    
当你必须要显式配置bean的时候（比如，有些源码不是由你来
维护的，而当你需要为这些代码配置bean的时候），我推荐使用类型
安全并且比XML更加强大的JavaConfig。
## 2.2　自动化装配bean
Spring从两个角度来实现自动化装配：
- 组件扫描（component scanning）：Spring会自动发现应用上下文中所创建的bean。
- 自动装配（autowiring）：Spring自动满足bean之间的依赖。
### 2.2.1　创建可被发现的bean  p67
**==@Component注解==**  
这个简单的注解表明该类会作为==组件类==，并告知Spring要为这个类创建
bean。  
当一个类使用了@Component注解，Spring会为你把事情处理妥当。     

我们还需要显式配置一下Spring，
从而命令它去寻找带有@Component注解的类，并为其创建bean。    
==@ComponentScan==注解启用了组件扫描

如果你更倾向于使用XML来启用组件扫描的话，那么可以使
用Spring context命名空间的<context:component-scan>元
素
### 2.2.2　为组件扫描的bean命名
Spring应用上下文中所有的bean都会给定一个ID，也就是将类名的第一个字母变为小写；      
如果想为这个bean设置不同的ID，你所要做的就是==将期望的ID作为值传递给@Component注解==    

还有另外一种为bean命名的方式，这种方式不使用@Component注解，而是使用Java依赖注入规范（Java Dependency Injection）中所提供的@Named注解来为bean设置ID：
### 2.2.3　设置组件扫描的基础包 p70

### 2.2.4　通过为bean添加注解实现自动装配 
>@Autowired注解 

自动装配就是让Spring自动满足bean依赖的一种方法，在满足依赖的过程中，会==在Spring应用上下文中寻找匹配某个bean需求的其他bean==。      

@Autowired注解不仅能够用在构造器上，还能可以用在类的任何方法上

如果没有匹配的bean，那么在应用上下文创建的时候，Spring会抛出一个异常。为了避免异常的出现，你可以将@Autowired的required属性设置为false：

如果有多个bean都能满足依赖关系的话，Spring将会抛出一个异常，表明没有明确指定要选择哪个bean进行自动装配。

==@Autowired是Spring特有的注解==。如果你不愿意在代码中到处使用
Spring的特定注解来完成自动装配任务的话，那么你可以考虑将其替换为@Inject：

### 2.2.5　验证自动装配

## 2.3　通过Java代码装配bean
尽管在很多场景下通过组件扫描和自动装配实现Spring的自动化配置
是更为推荐的方式，  

但==有时候自动化配置的方案行不通==，因此需要明
确配置Spring。  

==比如说，你想要将第三方库中的组件装配到你的应用中==，  
在这种情况下，是没有办法在它的类上添加@Component和
@Autowired注解的，因此就不能使用自动化装配的方案了。    

在进行显式配置的时候，有两种可选方案：Java和XML。

通过JavaConfig显式配置Spring
### 2.3.1　创建配置类

### 2.3.2　声明简单的bean p76
>@Bean注解

@Bean注解会告诉Spring这个方法将会返回一个对象，该对象要注册
为Spring应用上下文中的bean。方法体中包含了最终产生bean实例的
逻辑。
### 2.3.3　借助JavaConfig实现注入

## 2.4　通过XML装配bean
以前spring开发采用的方式    
### 2.4.1　创建XML配置规范 p80

借助Spring Tool Suite创建XML配置文件创建和管理Spring XML配置文件的一种简便方式是使用Spring Tool Suite

### 2.4.2　声明一个简单的<bean>
要在基于XML的Spring配置中声明一个bean   
<bean>元素类似于
JavaConfig中的@Bean注解

### 2.4.3　借助构造器注入初始化bean
在XML中声明DI时，会有多种可选的配置方案和风格。具体到
构造器注入，有两种基本的配置方案可供选择：
- <constructor-arg>元素
- 使用Spring 3.0所引入的c-命名空间
### 2.4.4　设置属性

### 2.4.5   导入与混合配置

## 2.5 导入与混合配置

## 2.6 小结
Spring框架的核心是Spring容器        
容器负责管理应用中组件的生命周期，它会创建这些组件并保证它们的依赖能够得到满足

Spring中装配bean的三种主要方式
- 自动化配置
- 基于Java的显式配置
- 基于XML的显式配置

这些技术都描述了Spring应用中的组件以及这些组件之间的关系



















