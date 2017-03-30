#####  ClassLoader(类加载器)
-   负责将Class加载到JVM中
-   审查每个类由谁加载，父优先的等级加载机制
-   将class字节码 重新解析成JVM统一要求的对象格式

### 6.1 ClassLoader类结构分析   p157
>   通过字节码生成JVM能识别的Class对象
    覆盖ClassLoader父类的findClass方法来实现类的加载规则，取得要加载类的字节码。
    调用defineClass方法生成类的Class对象，如果希望类被加载到JVM中后直接被链接（Link），可以继续调用resolveClass
    
>   如果不想重新定义加载类的规则，也没有复杂的处理逻辑，只想在运行时能加载自己制定的一个类，可以用 this.getClass().getClassLoader().loadClass("class") 调用ClassLoader的loadClass方法 来获取这个类的Class对象

>ClassLoader是个抽象类，它有许多子类， 我们如果要实现自己的ClassLoader，一般都会继承**URLClassLoader**这个子类，这个类已经帮我们实现了大部分的工作

>ClassLoader还提供了一些辅助方法，如获取class文件的方法 getResource、getResourceAsStream等

### 6.2 ClassLoader的等级加载机制
>整个JVM平台提供**三层ClassLoader**，这三层ClassLoader可以分为两种类型，可以理解为 接待室服务的接待室 和为会员服务的 接待室 两种
- Bootstrap ClassLoader,  

- ExtClassLoader，

- AppClassLoader,这个类加载器是专门为接待会员服务的，他的父类是ExtClassLoader。所有在System.getProperty("java.class.path")目录下的类都可以被这个类加载器加载，这个目录就是常用的 classpath

>ExtClassLoader和AppClassLoader 都位于sun.misc.Launcher类中，他们是Launcher的内部类

>ExtClassLoader和AppClassLoader都继承了URLClassLoader类，而URLClassLoader又实现了抽象类 ClassLoader, 在创建Launcher对象时会先创建ExtClassLoader，然后将ExtClassLoader 对象作为父加载器 创建 AppClassLoader对象， 而通过 Launcher.getClassLoader()方法获取的ClassLoader就是 APPClassLoader 对象。所以如果在Java应用中没有定义其他 ClassLoader,那么 除了 System.getProperty("java.ext.dirs")目录下的类是由 ExtClassLoader 加载外，其他类都由**AppClassLoader**来加载

###  JVM加载class文件到内存 有两种方式      p160
- 隐式加载：不通过在代码里调用 ClassLoader 来加载需要的类，而是通过JVM 来自动加载需要的类到内存的方式；例如，当我们在类中继承或者引用某各类时，JVM在解析当前这个类时发现引用的类不在内存中，那么就会自动将 这些类加载到内存中
- 显式加载：在代码中调用 ClassLoader类来加载一个类，例如 调用this.getClass().getClassLoader().loadClass()或者 Class.forName(), 或者我们自己实现的 ClassLoader的 findClass() 方法
> 这两种方式可以混合使用，例如 通过自定义的ClassLoader 显式加载 一个类，这个类又引用了其他类，这些其他的类可以是隐式加载的

### 6.3 如何加载class文件
>ClassLoader是如何加载一个class文件到JVM的？

    1、找到.class文件并把这个文件包含的字节码记载进内存
    2、字节码验证，Class类数据结构分析及相应的内存分配，符号表的链接
    3、类中静态属性和初始化赋值，静态块的执行
    
#### 6.3.1  加载字节码到内存
>抽象类ClassLoader中并没有定义如何去加载指定类 并把它的字节码记载到内存需要的子类中去实现； 子类 **URLClassLoader**实现 **findClass**()，通过**URLClassPath**类 帮助取得要加载的 class文件字节流，再读取它的byte字节流，最后通过调用**defineClass**()方法创建 类对象

> 必须要指定一个URL 数据才能创建 URLClassLoader对象，必须要指定这个ClassLoader默认到哪个目录下去查找class文件，这个URL数组是创建URLClassLoader对象的必要条件，从URLClassLoader的名字中可以发现它是通过URL的形式来表示 ClassPath路径的。

>在创建URLClassLoader对象时 会根据传过来的URL数组中的路径来判断是文件还是jar包，根据路径的不同分别创建 FileLoader 或者 JarLoader，或者使用默认的加载器。 当JVM调用findClass时由这几个加载器 来讲class文件的字节码加载进内存中

#### 如何设置每个ClassLoader的搜索路径，三个ClassLoader类型的参数形式            p162

#### 6.3.2  验证与解析
- 字节码验证
- 类准备
- 解析  

#### 6.3.3  初始化Class兑现
>在类中包含的静态初始化器都被执行，在这一阶段末尾 静态字段都被初始化为 默认值

### 6.4 常见加载类错误分析
> 在执行Java程序是遇到的 ClassNotFoundException 和 NoClassDefFoundError 两个异常，他们都和类加载有关
#### 6.4.1      ClassNotFoundException
>这个异常通常发生在显式加载类的时候，显式加载一个类通常有如下方式：
- 通过 类 Class 中的 forName() 方法。
- 通过 类 ClassLoader 中的 loadClass() 方法。
- 通过 类 ClassLoader 中的 findSystemClass() 方法
> 出现这种错误，是因为当 JVM要加载指定文件的字节码到内存时，并没有找到这个文件对应的字节码，也就是文件并不存在 ， 解决的办法是 检查当前 classpath 目录下有没有指定的文件存在， 如果不知道当前的 classpath 路径，可以通过如下命令获取
 this.getClass().getClassLoader().getResource("").toString()
 #### 6.4.2  NoClassDefFoundError
 >在JVM的规范中描述了出现 NoClassDefFoundError 可能的情况就是使用 new关键字、属性引用某个类、继承了某个接口或类，以及方法的某个参数中引用了某个类，这时会触发JVM 隐式加载这些类时 发现这些类不存在的异常
 
 >解决这个错误的办法 就是确保每个类 引用的类都在 当前的classpath 下面
 
 #### 6.4.3     UnsatisfiedLinkError
    这个异常通常是在JVM启动的时候，删除了 JVM中的某个lib
    
#### 6.4.4   ClassCastException
>这个错误通过在 程序中出现强制类型转换时 出现这个错误； JVM 在做类型转换时会 按照如下规则进行检查：
-  对于普通对象，对象必须是目标类的实例或 目标类的子类的实例。如果目标类是接口，那么会把它当做实现了该接口的一个子类
-  对于数组类型，目标类必须是数组类型 或 java.lang.Object、java.lang.Cloneable、java.io.Serializable
>如果不满足上面的规则，JVM就会报这个错误。避免这个错误有两种方式：
- 在容器类型中 显示地指明这个容器包含的对象类型，如 Map<String,Integer> m = new HashMap<Stirng,Integer>(), 这样上面的代码在编译阶段就会通过检查
- 先通过 instanceof 检查是不是目标类型，然后再进行强制类型转换
####  6.4.5   ExceptionInInitializerError

### 6.5 常用的ClassLoader 分析      p168
>前面分析了 ClassLoader的工作机制，下面看看 一些开源的框架是 如何根据这个工作机制来设置自己的ClassLoader的， 为什么要这么设置

>中间省略一万字     p168-172

### 6.6 如何实现自己的ClassLoader
>ClassLoader能够完成的事件有以下几种情况
- 在自定义路径下 查找自定义的class类文件， 因为我们需要的class文件并不总是 在已经设置好的 ClassPath下面， 必须自己实现一个ClassLoader去找到这个类
- 对我们自己要加载的 做**特殊处理**， 例如为了保证网络传输的类的安全性，可以将类经过加密后再传输，在加载到JVM 之前需要对类的字节码再解密， 这个过程可以在自定义的 ClassLoader中实现
- 可以定义类的实现机制， 例如 我们可以检查 已经加载的class文件已经被修改，如果修改了，可以重新加载这个类，从而实现类的热部署
#### 6.6.1  加载自定义路径下的class文件

#### 6.6.2  加载 自定义格式的 class文件     p174
>例如 从网络上下载一个 经过加密处理的class文件的字节码，客户端接收到字节码之后需要经过解码才能还原成 原始的类格式， 然后再通过ClassLoader的defineClass()方法创建这个类的实例， 最后完成类的加载

> 在方法 deCode()中可以对网络传输过来的字节码进行某种解密处理， 然后返回正确的class字节码， 调用defineClass()来创建 类对象

### 6.7 实现类的热部署      p176

### 6.8 Java应不应该动态加载类


### 6.9 总结
>本章介绍了 ClassLoader的基本工作机制，介绍了Tomcat等框架的ClassLoader的实现原理，最后介绍了如何创建自己的ClassLoader