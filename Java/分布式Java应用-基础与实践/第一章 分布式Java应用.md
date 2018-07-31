### 分布式Java应用  
大型应用的不同子系统位于不同JVM或者不同机器上，通过通信共同实现业务功能    

分布式Java应用通常有两种典型的方法来实现  
#### 1.  基于消息方式实现系统间的通信
系统间通信的消息可以是 
- 字节流
- 字节数组
- Java对象    

其他系统收到消息后进行相应的业务处理。  

消息形式的系统间通信通常基于网络协议实现，如 TCP/IP 和 UDP/IP       

TCP/IP 和 UDP/IP可以完成数据的传输，但是要完成系统间通信，还需要对数据进行处理。例如读取和写入数据，按照POSIX标准分为==同步IO和异步IO==     

同步IO最常用的是BIO 和 NIO，
- BIO就是发起IO的读或写操作时，均为==阻塞方式==，只有当程序读到了流或将流写入操作系统后，才会释放资源    
- NIO是基于事件驱动思想的，当发起IO的读或写操作时，是非阻塞的，==当Socket有流可读或可写入Socket时，操作系统会相应地通知应用程序进行处理==，应用再将流读取到缓冲区或写入操作系统    

对于网络IO，主要有连接建立，流读取及流写入 三种事件     

AIO为异步IO方式，同样基于事件驱动思想

#### 2. 基于远程调用方式实现系统间的通信    
通过调用本地的一个Java接口的方法，透明地调用远程的Java实现，具体细节有Java或框架完成，这种方式在Java中主要用例实现基于 RMI 和 WebService的应用  

## 1.1 基于消息方式实现系统间的通信
### 1.1.1 基于Java自身技术实现消息方式的系统间通信  
基于Java自身包实现消息方式的系统间通信方案有 
- TCP/IP + BIO
- TCP/IP + NIO


- UDP/IP + BIO
- UDP/IP + NIO

### TCP/IP + BIO
Java中可基于 Socket、ServerSocket来实现 TCP/IP+BIO的系统间通信，==Socket用于实现建立连接及网络IO的操作==，==ServerSocket用于实现服务端端口的监听及Socket对象的获取==    

在实际的系统中，通常要面对的是 客户端同时发送多个请求到服务器端，服务器端同时要接收多个连接发送的请求  

为了满足客户端能同时发送多个请求到服务端，最简单的方法就是 生成多个 Socket，    
但是多个Socket会消耗过多的本地资源，而且生成Socket比较慢，频繁创建会导致系统性能不足    
通常采用==连接池==来维护Socket是比较好的    

但是连接池中Socket个数有限，这时要合理控制==等待响应的超时时间==，对于TCP/IP+BIO的方式，可采用Socket.setSoTimeout来设置响应的超时时间    

TCP/IP+BIO的方式为了能满足服务器端同时接受多个连接发送的请求，通常在 accept获取Socket后，将Socket放入一个线程中处理，==一连接一线程==，缺点是，请求过多时会导致服务器端资源耗尽，==必须限制创建的线程数量==  

### TCP/IP+NIO
通过 java.nio.channels中的Channel和Selector的相关类可以实现 TCP/IP+NIO方式的系统间通信   
Channel有SocketChannel 和 ServerSocketChannel两种：
- SocketChannel用于建立连接、监听事件及操作读写，
- ServerSocketChannel用于监听端口及监听连接事件 

程序通过 ==Selector来获取是否有要处理的事件==   

NIO 是典型的 Reactor模式的实现，通过注册感兴趣的事件及扫描是否有感兴趣的事件发生，从而做相应的动作  

对于服务器端接收多个连接请求的情况，通过一个线程来专门监听连接的事件，另一个或多个线程监听网络流读写的事件。    

当有实际的网络流读写事件发生后，再放入线程池中处理。这个方式比 TCP/IP+BIO的好处在于可接收。。。

==一请求一线程==

对于高访问量的系统，TCP/IP+NIO 结合一定的改造在客户端能够带来更高的性能，在服务端能支撑更高的连接数

### UDP/IP + BIO
Java 对 UDP/IP 方式的网络数据传输同样采用 Socket机制，只是 UDP/IP下的Socket没有建立连接的要求，由于无连接，因此无法进行双向通信，如果要求双向通信，必须两端都成为 UDP Server   
在Java中可基于 DatagramSocket 和 DatagramPacket 实现 UDP/IP+BIO 方式的系统间通信 

DatagramSocket 负责 监听端口及读写数据，DatagramPacket 作为数据流对象进行传输

### UDP/IP+NIO
Java可通过 DatagramChannel 和 ByteBuffer 来实现 UDP/IP+NIO方式的系统间通信  

DatagramChannel 负责监听端口及进行读写，ByteBuffer 则用于数据流传输    

在Java中 可基于 MulticastSocket 和 DatagramPacket 来实现多播网络通信  


**** 为了让开发人员 能更加专注于对数据进行业务处理，而不用过多关注纯技术细节，出现了基于各种通信协议的 系统间通信的框架，比如==Mina==

### 1.1.2 基于开源框架实现消息方式的系统间通信  p10
Mina 是 Apache的顶级项目，基于Java NIO 构建，同时支持 TCP/IP和UDP/IP两种协议，Mina对外屏蔽了Java NIO使用的复杂性，并在性能上做了不少的优化   

Jboss Netty也是广受关注的 Java通信框架

## 1.2 基于远程调用方式实现 系统间的通信
远程调用方式尽可能使得分布式系统间通信和系统内一样，但是远程调用存在一系列问题需要注意
- 网络
- 超时
- 序列化/反序列化
- 调试

### 1.2.1 基于Java自身技术实现远程调用方式的 系统间通信 
在Java中实现 远程调用方式的技术 主要有 RMI和WebService两种 
### RMI
- 
-

### WebService  p16
- 
- 
### 1.2.2 基于开源框架实现远程调用方式的系统间通信  
### Spring RMI
-
-
### CXF
-
-
---
> 总结：在大型的应用中，分布式的概念不仅仅是 跨JVM或跨机器的系统间通信，还会涵盖如 集群，负载均衡、分布式文件系统、分布式缓存等多方面知识



