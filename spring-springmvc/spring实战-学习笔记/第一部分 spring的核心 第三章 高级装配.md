## 3.1 环境与profile
### 3.1.1 配置profile bean
在Java配置中，可以使用@Profile注解指定某个bean属于哪一个
profile。

在Spring 3.1中，只能在类级别上使用@Profile注解。不过，从
Spring 3.2开始，你也可以在方法级别上使用@Profile注解，
与@Bean注解一同使用。这样的话，就能将这两个bean的声明放到同
一个配置类之中

也可以通过<beans>元素的profile属性，在XML中配置
profile bean。
### 3.1.2 激活profile
Spring在确定哪个profile处于激活状态时，需要依赖两个独立的属
性：spring.profiles.active和spring.profiles.default。

有多种方式来设置这两个属性：
- 作为DispatcherServlet的初始化参数；
- 作为Web应用的上下文参数；
- 作为JNDI条目；
- 作为环境变量；
- 作为JVM的系统属性；
- 在集成测试类上，使用@ActiveProfiles注解设置。

> p111
Spring提供了@ActiveProfiles注解，我们可以使用它来指定运行
测试时要激活哪个profile

在条件化创建bean方面，Spring的profile机制是一种很棒的方法，这里的条件要基于哪个profile处于激活状态来判断
## 3.2　条件化的bean
Spring 4引入了一个新的@Conditional注解，它可以用到带有@Bean注解的方法上。如果给定的条件计算结果为true，就会创建这个bean，否则的话，这个bean会被忽略。
## 3.3　处理自动装配的歧义性
当确实发生歧义性的时候，Spring提供了多种可选方案来解决
这样的问题。
- 可以将可选bean中的某一个设为首选（primary）的bean，
- 或者使用限定符（qualifier）来帮助Spring将可选的bean的范围缩小到只有一个bean。

### 3.3.1　标示首选的bean
在Spring中，可以通过@Primary来表达最喜欢的方案。        

@Primary能够与@Component组合用在组件
扫描的bean上，也可以与@Bean组合用在Java配置的bean声明中

如果你使用XML配置bean的话，同样可以实现这样的功能。<bean>
元素有一个primary属性用来指定首选的bean

### 3.3.2　限定自动装配的bean
@Qualifier注解是使用限定符的主要方式。它可以与@Autowired
和@Inject协同使用，在注入的时候指定想要注入进去的是哪个
bean
#### 创建自定义的限定符
我们可以为bean设置自己的限定符，而不是依赖于将bean ID作为限定符
#### 使用自定义的限定符注解 p120

## 3.4　bean的作用域
在默认情况下，Spring应用上下文中所有bean都是作为以单例
（singleton）的形式创建的。也就是说，不管给定的一个bean被注入到其他bean多少次，每次所注入的都是同一个实例。

Spring定义了多种作用域，可以基于这些作用域创建bean，包括：
- 单例（Singleton）：在整个应用中，只创建bean的一个实例。
- 原型（Prototype）：每次注入或者通过Spring应用上下文获取的
时候，都会创建一个新的bean实例。
- 会话（Session）：在Web应用中，为每个会话创建一个bean实
例。
- 请求（Rquest）：在Web应用中，为每个请求创建一个bean实
例。

单例是默认的作用域，但是正如之前所述，对于易变的类型，这并不
合适。如果==选择其他的作用域==，要==使用@Scope注解==，它可以
与@Component或@Bean一起使用。

### 3.4.1 使用会话和请求作用域 p124

### 3.4.2　在XML中声明作用域代理

## 3.5　运行时值注入

### 3.5.1　注入外部的值

### 3.5.2　使用Spring表达式语言进行装配 p132

## 3.6 小结  p140
























