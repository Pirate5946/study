### 1.2　Java技术体系
从广义上讲，Clojure、JRuby、Groovy等运行于Java虚拟机上的语言及其相关的程序都
属于Java技术体系中的一员。      
如果仅从传统意义上来看，Sun官方所定义的Java技术体系包括
以下几个组成部分：
- Java程序设计语言
- 各种硬件平台上的Java虚拟机
- Class文件格式
- Java API类库
- 来自商业机构和开源社区的第三方Java类库
>我们可以把==Java程序设计语言==、==Java虚拟机==、==Java API类库==这三部分统称为==JDK==（Java
Development Kit），JDK是用于支持Java程序开发的最小环境

如果按照技术所服务的领域来划分，或
者说按照Java技术关注的重点业务领域来划分，Java技术体系可以分为4个平台，分别为
- Java Card：支持一些Java小程序（Applets）运行在小内存设备（如智能卡）上的平台。
- Java ME（Micro Edition）：支持Java程序运行在移动终端（手机、PDA）上的平台，对
Java API有所精简，并加入了针对移动终端的支持，这个版本以前称为J2ME。
- Java SE（Standard Edition）：支持面向桌面级应用（如Windows下的应用程序）的Java
平台，提供了完整的Java核心API，这个版本以前称为J2SE。
- Java EE（Enterprise Edition）：支持使用多层架构的企业应用（如ERP、CRM应用）的
Java平台，除了提供Java SE API外，还对其做了大量的扩充[3]并提供了相关的部署支持，这
个版本以前称为J2EE。
### Sun HotSpot VM
提起HotSpot VM，相信所有Java程序员都知道，它是Sun JDK和OpenJDK中所带的虚拟
机，也是目前使用范围最广的Java虚拟机。

>图　1-4　可以运行在JVM之上的语言 p38

### 1.5.3　多核并行  p40
如今，CPU硬件的发展方向已经从高频率转变为多核心，随着多核时代的来临，软件开
发越来越关注并行编程的领域。    
早在JDK 1.5就已经引入java.util.concurrent包实现了一个粗粒
度的并发框架。  
而JDK 1.7中加入的java.util.concurrent.forkjoin包则是对这个框架的一次重要
扩充。  
Fork/Join模式是处理并行编程的一个经典方法，如图1-5所示。虽然不能解决所有的问
题，但是在此模式的适用范围之内，能够轻松地利用多个CPU核心提供的计算资源来协作完
成一个复杂的计算任务。  
通过利用Fork/Join模式，我们能够更加顺畅地过渡到多核时代。

在Java 8中，将会提供Lambda支持，这将会极大改善目前Java语言不适合函数式编程的
现状（目前Java语言使用函数式编程并不是不可以，只是会显得很臃肿），  
函数式编程的一
个重要优点就是这样的程序天然地适合并行运行，这对Java语言在多核时代继续保持主流语
言的地位有很大帮助。        
在JDK外围，也出现了专为满足并行计算需求的计算框架，如Apache的Hadoop
Map/Reduce，这是一个简单易懂的并行框架，能够运行在由上千个商用机器组成的大型集群
上，并且能以一种可靠的容错方式并行处理TB级别的数据集。  
另外，还出现了诸如Scala、
Clojure及Erlang等天生就具备并行计算能力的语言。

### 1.5.5　64位虚拟机

### 1.6　实战：自己编译JDK  p43
笔者下载的是OpenJDK 7
Update 6 Build b21版源码包，2012年8月28日发布，大概99MB，解压后约为339MB。
### 1.6.3　构建编译环境 p47

### 1.6.5　在IDE工具中进行源码调试 p51