## 第五章 坐标和依赖
### 5.1 何为Maven坐标
Maven定义了一组规则：任何一个构件都可以使用Maven坐标 唯一标识      
>Maven坐标的元素包括 
- groupid
- artifactId
- version
- packaging
- classifier

Maven内置了一个中央仓库地址，包含了大部分流行的开源项目组件

每个项目都要定义适当的坐标，这样其他项目才能引用该项目的组件
### 5.2 坐标详解
- groupid   （必选）    
定义当前Maven项目隶属的实际项目
- artifactId    （必选）    
定义实际项目中的一个模块，推荐使用实际项目作为artifactId 的前缀
- version   （必选）    
定义Maven项目当前所处的版本
- packaging   （可选）  
定义Maven项目的打包方式，默认是 jar
- classifier    （不能直接定义）    
定义构建输出的附属构件



### 5.4 依赖的配置 p81
- groupid、artifactId、version 依赖的基本坐标
- type 依赖的类型
- scope 依赖的范围 
- optional 标记依赖是否可选
- exclusions 用来排除传递性依赖

### 5.5 依赖的范围
依赖范围用来控制依赖于三种classpath的关系
- 编译classpath
- 测试classpath
- 运行classpath

Maven有以下几种依赖范围
- compile  默认的依赖范围
- test
- provided
- runtime
- system
### 5.6 传递性依赖

### 5.7 依赖调解 p85
第一原则：路径最近者优先    
第二原则：第一声明者优先

### 5.9 最佳实践
#### 5.9.1 排除依赖

#### 5.9.2 归类依赖

#### 5.9.3 优化依赖
查看当前项目下已解析依赖
```
mvn dependency:list
```
查看当前项目的依赖树
```
mvn dependency:tree
```
分析当前项目的依赖
```
mvn dependency:analyze
```
### 5.10 小节 p93
本章介绍了Maven的两个核心概念:坐标和依赖    
## 第六章 仓库
在Maven中，任何一个依赖，插件或者项目构建的输出，都可以成为构件

任何一个构件都有一组坐标唯一标识

Maven可以在某个位置统一存储所有Maven项目共享的构件，这个统一的位置就是 仓库

### 6.2 仓库的布局
任何一个构件都有其唯一的坐标，根据这个坐标可以定义其在仓库中的唯一存储路径      
#### 解析某个依赖的仓库路径的源码 p95
### 6.3 仓库的分类
本地仓库和远程仓库  
- Maven根据坐标寻找构件的时候，先去本地仓库，没有再起远程仓库，都没有就报错
- 远程仓库的分类
    - Maven核心自带的远程仓库
    - 局域网内私有的仓库服务器
    - 其他公开的远程仓库
> 我的Maven本地仓库 C:\Users\lt\.m2\repository    
F:\maven_repo

>全局配置文件地址 C:\dev_tools\apache-maven-3.3.9\conf

> 将构件安装到本地仓库的方式
- 依赖Maven从远程仓库下载到本地仓库中
- 将本地项目的构件安装到Maven仓库
```
mvn clean install
```
### 6.3.2 远程仓库
安装好Maven，当用户输入第一条Maven命令之后，Maven才会创建本地仓库，然后根据配置和需要，从远程仓库下载构件至本地仓库

每个用户只能有一个本地仓库，但是可以配置访问多个远程仓库
### 6.3.3 中央仓库
中央仓库是默认的远程仓库
配置文件 在 $M2_HOME/lib/maven-2.2.1-uber.jar的org/apache/maven/model/pom-4.0.0.xml       
中央仓库（==http://repo1.maven.org/maven2==）包含了绝大多数流行的开源Java组件，以及源码
### 6.3.4 私服
架设在局域网内的仓库服务，代理广域网的远程仓库，私服没有的构件再去外部的远程仓库下载        
使用Maven私服的好处
- 节省外网带宽
- 加速Maven构建
- 部署不方便从外部远程仓库获取的第三方构件
- 提供稳定性，增强控制
- 降低中仓库的负荷

[使用Nexus搭建Maven私服](http://www.cnblogs.com/quanyongan/archive/2013/04/24/3037589.html)
## 6.4 远程仓库的配置 p101
配置 pom.xml    
在repositories元素下使用repository子元素声明一个或者多个远程仓库  
重要属性    
- relaeses 控制发布版本构件的下载
- snapshots 控制快照版构件的下载
    - updatePolicy 检查更新频率
    - checksumPolicy 检查校验和文件的策略
### 6.4.1 远程仓库的认证
配置setting.xml
```
  <servers>
  	<server>
  		<id>mvn-mas</id>
  		<username>wangwen</username>
  		<password>12345678</password>
  	</server>
```
id元素必须和POM中需要认证的repository元素的id完全一致

### 6.4.2 部署至远程仓库
Maven除了能对项目进行编译，测试，打包之外，还能将项目生成的构件部署到仓库中  

配置 
- distrubutuinManagement元素的子元素 
    - repository
    - snapshotRepository

分别配置 id,name,url；==id为该远程仓库的唯一标识==，url为该仓库的地址

对应远程仓库的认证配置为 setting.xml的server元素，其id与仓库的id匹配，并配置正确的认证信息  

配置完成后执行命令将项目构建的输出的构件部署到对应的远程仓库
```
mvn clean deploy
```
## 6.5 快照版本 p105
## 6.6 从仓库解析依赖的机制
强制检查更新
```
mvn clean update -U
```
仓库元数据并不是永远正确的，当无法解析某些构件，或者解析到错误构件，可能是因为仓库元数据出现错误，这时需要手工，或者使用工具 如 Nexus进行修复
## 6.7 镜像
如果仓库a可以提供仓库b存储的所有内容，可以认为a是b的一个镜像  
中央仓库在中国的镜像 http://maven.net.cn/content/groups/public  

可以配置Maven使用该镜像来代替中央仓库，编辑 setting.xml
```
<settings>
    ...
    <mirrors>
        <mirror>
            <id>maven.net.cn</id>
            <name>one of the central mirrors in China </name>
            <url>http://maven.net.cn/content/groups/public/</url>
        </mirror>
    </mirrors>
```
关于镜像的一个更为常见的用法是结合私服 p109

## 6.8 仓库搜索服务

## 6.9 小结

# 第七章 生命周期和插件
除了坐标、依赖以及仓库之外，Maven另外两个核心概念 是 生命周期和插件      
Maven的生命周期是为了对所有的构建过程进行抽象和统一 

Maven从大量项目和构件工具中学习和反思，总结了一套高度完善，易扩展的项目周期，包括了
- 清理，初始化
- 编译，测试
- 打包，集成测试
- 验证，部署，站点生成  
等几乎所有的构建步骤，都能映射到一个生命周期上  

Maven的思想类似 设计模式中的 模板方法；即在 父类中定义算法的整体结构，子类可以通过实现或者重写父类的方法来控制实际行为

Maven的生命周期都是抽象的，实际的动作（如编译源代码）都交由插件来完成

Maven拥有三套相互独立的生命周期，
- clean  清理项目
- default 构建项目
- site 建立项目站点

> clean 生命周期 

包含三个阶段
- 
> default生命周期   p116

> site 生命周期

### 7.2.5 命令行与生命周期
一条mvn命令可能包含 一个或多个maven生命周期的一个或多个阶段   
```
mvn clean install
调用clean生命周期的clean阶段和default生命周期的install阶段
```
## 7.3 插件目标
一个插件可能有多个功能，一个功能称为一个插件目标    

maven-dependency-plugin有十多个目标
- dependency:analyze  分析项目依赖
- dependency:tree   列出项目依赖树

冒号前面是插件前缀，后面是该插件的目标
## 7.4 插件绑定
Maven的==生命周期阶段==与==插件的目标==相互绑定，完成实际的构建任务        
例如 项目编译这个实际任务 对应 default生命周期的compile阶段， 同时对应 maven-compiler-plugin插件的compile目标

由于项目的打包类型会影响构建的具体过程，因此 default生命周期的阶段与插件目标的绑定关系 由POM中的packaging元素定义的打包类型决定，默认的打包类型是 jar

### 7.4.2 自定义绑定  p121
查看插件详细信息，了解插件目标的默认绑定阶段
```
maven-help：describe-Dplugin = org.apache.maven.plugins:maven-source-plugin:2.1.1-Ddetail
```
## 7.5 插件配置
完成==插件和生命周期的绑定== 之后，用户还可以配置插件目标的参数（通过命令行和POM配置）
### 7.5.1 命令行插件配置
在Maven命令中使用 -D参数，并伴随一个参数键=参数值的形式，来配置插件目标的参数        

### 7.5.2 POM中插件全局配置

### 7.5.3 POM中插件任务配置

## 7.6 获取插件信息 
使用正确的插件并进行正确的配置
### 7.6.1 在线插件信息
附录C 归纳了一些常用插件
### 7.6.2 使用 maven-help-plugin描述插件  p127

## 7.7 从命令行调用插件

## 7.8 插件解析机制 p129

## 7.9 小结  p133
介绍了 Maven的生命周期和插件 这两个重要概念,阐述了 
- clean,
- default,
- site  
三套生命周期,介绍了 Maven插件与生命周期绑定,配置插件,获取插件信息

# 第八章 聚合与继承  p134
maven的集合特性能够把 项目的各个模块聚合在一起构建，而maven的继承特性能够 抽取各模块相同的依赖和插件等配置 ，在简化POM 的同时，促进各个模块配置的一致性    

## 8.1 举例显示 spring代码模块结构

测试用例只测试接口，不测试实现，测试代码不能引用实现类

## 8.2 聚合（多模块）  p143
- 对于聚合模块来说，它的<packaging>的值必须为 pom，否则无法构建       
- <modules> 实现聚合的核心配置，通过在<modules>中声明任意数量的module元素来实现模块的聚合，每个module的值都是一个当前POM的相对目录    
- 聚合模块的内容仅是一个pom.xml，它不像其他模块有 src/main/java, src/test/java等目录，聚合模块仅仅是帮助其他模块构建的工具，本身并无实质内容
- 聚合模块与其他模块也可以是平行关系


## 8.3 继承 p146


# 第九章 使用Nexus创建私服  p167


# 第十章 使用Maven进行测试 p191


# 第十二章 使用Maven构建Web应用  p240

# 第十三章 版本管理 p262

# 第十四章 灵活的构建 p278

# ==十六章== m2eclipse p322

### 16.3.1 导入本地Maven项目 























