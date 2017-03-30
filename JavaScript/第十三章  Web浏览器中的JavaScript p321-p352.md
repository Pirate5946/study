### 13.1 客户端JavaScript
>==Window对象==是所有客户端JavaScript特性和API的主要接入点     
Window对象定义了一些属性，比如Location对象的location属性，Location对象指定当前显示在窗口中的URL，并允许脚本往窗口载入新的URL

>Window对象中比较重要的属性有 document，它表示显示在窗口中的文档    
每个Element对象都有style和className属性

>Window、Document、Element对象上另一个重要的属性集合 是 事件处理程序相关 的属性，可以为这些对象绑定一个 事件函数

事件处理程序可以让JavaScript代码 修改窗口、文档和组成文档的元素

>Window对象的onload处理程序是最重要的事件处理程序之一  ，当显示在窗口里的文档内容稳定并可以操作时触发它，JavaScript代码通常封装在 onload 事件中

#### 13.1.1 Web文档里的JavaScript
JavaScript程序可以通过 Document对象和它包含的 Element对象 遍历和管理文档内容。 它可以通过操纵CSS样式和类，修改文档内容的呈现。 并且可以通过 事件函数 定义文档元素的行为 

#### 13.1.2 Web应用里的JavaScript
> Web应用中不仅会用操作文档，还依赖Web浏览器环境提供更多服务

> ==Web浏览器已经是一个简易的操作系统了==

> XMLHttpRequest对象，可以对 HTTP请求编程 来启用网络， Web应用从服务器获取 新信息，而不用重新载入 页面

### 13.2 在 HTML里嵌入JavaScript
> 在HTML文档里嵌入客户端JavaScript代码有 4种方法
- 内联，放置在<script>和</script>标签对之间
- 放置在由<script>标签的src属性指定的外部文件中
- 放在HTML事件中，该事件由 onclick或 onmouseover这样的HTML属性值指定
- 放在一个URL里，这个URL使用特殊的 “javaScript:”协议

JavaScript最好通过  <script>标签的src属性指定的外部文件中

#### 13.2.3 脚本类型
>JavaScript是Web的原始脚本语言，默认<script>元素包含和引用javas代码，如果要使用不标准的 脚本语言，必须用 type属性 指定脚本的 MIME类型

    <script type="text/vbscript">
        //这里是 VBScript代码
    </script>
        
#### 13.2.5 URL中的JavaScript
> 在调试JavaScript代码或者 把这样链接硬编码 另存为 在任何页面上运行的书签时 可以这样做

### 13.3 JavaScript程序的执行 
> 如果Web页面包含一个嵌入的窗体（通常使用<iframe>元素），嵌入文档的JavaScript代码和被嵌入文档的JavaScript会有不同的全局对象   

> JavaScript程序执行有两个阶段 ，   
第一阶段：载入文档内容时会执行<script>元素里的代码，按照文档顺序从上往下执行     
第二阶段：这个阶段是异步的，而且由事件驱动

事件驱动阶段里 发生的第一个事件是load事件，指示文档已经完全载入，并可以操作

#### 13.3.1 同步、异步和延迟的脚本

#### 13.3.2 事件驱动的JavaScript
>注册事件最简单的办法是把 JavaScript函数赋值给 目标对象的属性   
    
    window.onload = function(){...};
    document.getElementById("button1").onclick = function(){...};
    
    function handleResponse(){...}
    request.onreadystatechange = handleResponse;
    
>如果需要为一个事件注册多个事件处理函数， 可以使用 addEventListaner() 方法，它允许在对象上注册多个 监听器

    window.addEventListaner("load",function(){...},false);
    IE8以及之前版本使用 attachEvent()
    
#### 13.3.3 客户端JavaScript线程模型
>可以使用 setTimeout()和setInterval()方法在后台运行子任务，同时更新 一个进度指示器向用户显示反馈
#### 13.3.4 ==客户端JavaScript时间线==    p337
1、
2、
3、
4、
5、
6、
7、
8、
### 13.4 兼容性和互用性
#### 13.4.6  IE中的条件注释

    <!--[if IE 6]>
        //
    <![end if]-->
    
### 13.6 安全性  p346
#### 13.6.1 JavaScript不能做什么

#### 13.6.1 同源策略

#### 13.6.4 跨站脚本
>  跨站脚本攻击

> 拒绝服务攻击

### 13.7 客户端框架