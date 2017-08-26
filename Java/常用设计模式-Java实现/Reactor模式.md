[Reactor模式究竟是个什么东西呢](http://ifeve.com/netty-reactor-4/)

## 1 Netty、NIO、多线程？
理清NIO与Netty的关系之前，我们必须先要来看看Reactor模式。   

Netty是一个典型的多线程的Reactor模式的使用，理解了这部分，在宏观上理解Netty的NIO及多线程部分就不会有什么困难了。

本篇文章依然针对Netty 3.7，不过因为也看过一点Netty 5的源码，所以会有一点介绍。



## 2
### 1、Reactor的由来

应用业务向一个中间人注册一个回调（event handler），当IO就绪后，就这个中间人产生一个事件，并通知此handler进行处理。 

这种回调的方式，也体现了“好莱坞原则”（Hollywood principle）-“Don’t call us, we’ll call you”，在我们熟悉的IoC中也有用到

好了，我们现在来看Reactor模式。在前面事件驱动的例子里有个问题：我们如何知道IO就绪这个事件，谁来充当这个中间人？  

Reactor模式的答案是：==由一个不断等待和循环的单独进程（线程）来做这件事==，    
它接受所有handler的注册，并负责==先操作系统查询IO是否就绪，在就绪后就调用指定handler进行处理==，这个角色的名字就叫做Reactor。

### 2、Reactor与NIO
Java中的NIO可以很好的和Reactor模式结合  

NIO中Reactor的核心是Selector，我写了一个简单的Reactor示例，这里我贴一个核心的Reactor的循环（这种循环结构又叫做EventLoop） 

### 3、与Reactor相关的其他概念
Reactor模式里，操作系统只负责通知IO就绪，具体的IO操作（例如读写）仍然是要在业务进程里阻塞的去做的，    

而Proactor模式则更进一步，由操作系统将IO操作执行好（例如读取，会将数据直接读到内存buffer中），而handler只负责处理自己的逻辑，真正做到了IO与程序处理异步执行。  

所以我们一般又说Reactor是同步IO，Proactor是异步IO。

