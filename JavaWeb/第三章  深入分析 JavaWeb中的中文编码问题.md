1、Java编码问题出现的根本原因
2、Java常见编码格式的区别
3、在Java中需要编码的场景，开发JavaWeb程序可能存在编码的地方
4、HTTP请求怎么控制编码格式，如何避免中文编码问题

### 3.1	几种常见的编码格式					p62
#### 3.1.1	为什么要编码
1、在

### 3.2	在Java中需要编码的场景				p64
#### 3.2.1	在I/O操作中存在的编码
>涉及编码的地方一般都是在 字符到字节的转换 或者 字节到字符的转换，需要转换的场景主要是I/O

Reader类是在Java的I/O中 读字符的父类，而InputStream类则是读字节的父类， InputStreamReader类就是关联 字节到字符 的桥梁，它负责在I/O中处理 读字节到字符的转换，而对字节到字符的 解码实现，InputStreamReader委托了StreamDecoder类去做，在StreamDecoder解码过程中 必须由用户指定Charset编码格式，没有指定则使用本地环境的默认字符集，中文默认GBK

写的情况也类似，写字符的父类是 Write，写字节的父类是OutputStream，通过OutputStreamWrite转换字符到字节，同样，StreamEncoder类负责将字符编码成字节

在I/O操作时，注意统一编解码的Charset	字符集

#### 3.2.2	在内存操作中编码					p66
Charset类通过 forname 设置编解码字符集，通过encode完成char[]到byte[]的编码，通过decode完成byte[]到char[]的解码；


### 3.3 	在Java中如何编解码				p68
根据CharsetName找到Charset类，根据这个字符集编码生成CharsetEncoder 这个类是所有字符编码的父类，调用encode方法实现不同字符编码集的编码


#### 3.3.1	按照 ISO-8859-1编码				p70
#### 3.3.2	按照GB2312编码		编码规则详解
#### 3.3.3	按照GBK编码		兼容GB2312
#### 3.3.4	按照UTF-16编码	
#### 3.3.5	按照UTF-8编码		编码规则详解		p73
#### 3.3.7	几种编码格式的比较					p74


### 3.4 	在Java Web中涉及的编解码			p75  ****
大部分I/O引起的乱码都是网络I/O，数据经过网络传输时是以字节为单位的，所以所有的数据必须能够被序列化为字节，Java中数据要被序列化，必须继承Serializable 接口

用户从浏览器发起一个HTTP请求，需要存在编码的地方是URL、Cookie、Parameter，服务器端接收到HTTP请求后要解析HTTP，其中URL、Cookie和表单参数需要解码

Servlet处理完所有请求的数据后，需要将这些数据再编码，通过Socket发送到浏览器，再通过浏览器解码成文本

#### 3.4.1	URL的编解码				p76		*****
URL编码规范
Tomcat在接收到浏览器端URL是如何解码的？			p77
在应用程序中，应该尽量避免在URL中使用非ASCII字符，不然可能碰到乱码问题
在服务器端最好设置<Connector/>中的URIEncoding和useBodyEncodingForURI这两个参数

#### 3.4.2	HTTP Header的编解码			p79

#### 3.4.3	POST表单的编解码				p80
POST表单提交的参数在第一次解码时调用的是requset.getParameter()，在此之前调用 request.setCharacterEncoding(charset)来设置编码格式，如果没有设置，表单提交的数据将会按照系统默认的解码方式解析

#### 3.4.4	HTTP BODY的编解码
Web客户端请求的资源获取后，将通过Response返回给客户端，这个过程要先经过编码，再由客户端进行解码

编解码字符集可以通过 response.setCharacterEncoding来设置，它会覆盖request.setCharacterEncoding(charset)，并通过Header的Content-Type返回客户端，客户端接收到Socket流时，将通过Content-Type的charset来解码， 

如果返回的HTTP Header中Content-Type没有设置charset，浏览器将根据HTML的

    <meta HTTP-equiv="Content-Type" content="text/html"; charset=GBK" />
中的charset来解码，如果没有定义，浏览器将通过默认的编码来解码

访问数据库都是通过 客户端JDBC驱动完成的，用JDBC存取数据时要与数据的内容编码保持一致，可以通过设置JDBC URL来指定

### 3.5		在JS中的编码问题		P82
#### 3.5.1	引入外部JS 文件
在HTML中引入外部JS时，如果JS的编码格式与当前页面的编码格式一致，那么不用设置JS的编码格式，如果不一致，那么需要指定<script src="" charset""></script>，与页面编码保持一致

#### 3.5.2	JS的URL编码			
通过JS发起的 异步调用的 URL 的默认编码受浏览器影响，在JS中处理URL 编码的函数主要有三个

1、esacape    在ECMAScript v3标准中已剔除

2、encodeURI
将URL中字符进行UTF-8的编码（特殊字符除外），每个码值前加上%，解码通过decodeURI函数

3、encodeURLComponent			p83
解码用 decodeURLComponent

4、Java和JS编解码问题				p83
Java端处理URL编解码有两个类，分别是java.net.URLEncoder和java.net.URLDecoder.

#### 3.5.3	其他需要编码的地方
除了URL和参数编码问题，在服务端还有很多地方可能存在编码，如可能需要读取XML、Velocity模板引擎、JSP或者从数据库读取数据

XML文件可以通过设置头来制定编码格式

    <?xml Version="1.0" encoding="UTF-8"?>
JSP设置编码如下
    
    <%@page contentType="text/html; charset=UTF-8"%>

### 3.6	常见问题分析			p85
出现乱码问题是因为 在从char到byte 和 byte和char 的转换中 编码和解码的字符集不一致导致的
#### 3.6.1		中文变成看不懂的字符
字符串在解码时用的字符集和编码时用的字符集不一致导致乱码，一个汉字字符变成了两个乱码字符
#### 3.6.2		中文变成一个问号
将中文或者中文符号用ISO-8859-1编码时，遇到不在码值范围内的字符统一会用3f表示，也就是？
#### 3.6.3		中文变成两个问号
#### 3.6.4		一种 不正常的正确编码		p86

### 3.7	一种繁简转换转换的实现方式		p87


### 3.8	总结					p88
1、常见编码的区别

2、几种支持中文的编码格式

3、Java中设计的编码问题及处理方式

4、网络I/O 可能遇到的编码问题

5、Tomcat对HTTP的解析

搞清楚哪里会有字节到字符的编码，或者字符到字节的编码




