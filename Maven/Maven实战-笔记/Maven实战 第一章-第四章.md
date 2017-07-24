### 1.1 何为maven
跨平台的项目管理工具，主要服务于基于Java平台的项目构建、依赖管理和项目信息管理
#### 1.1.1 何为构建
编译、运行单元测试、生成文档、打包和部署等就是构建
#### 1.1.2 maven是优秀的构建工具

#### 1.1.3 
Maven提供了一个中央仓库，能自动下载构件

### 1.2 为什么需要 Maven 

#### 1.2.3  Make
Make也许是最早的构建工具，由一个Makefile脚本文件驱动，使用Make自己定义的语法格式，与操作系统绑定，无法跨平台       
基本组成部分是一系列的规则      
每一条规则包括目标、依赖和命令
#### 1.2.4 Ant
Java版本的Make，Ant有一个构建脚本 build.xml     
bulid.xml的基本机构也是 目标、依赖和实现目标的任务      

##### 1.2.5 不重复发明轮子

### 1.3 Maven与极限编程

### 1.5 小结

## 第二章 maven的安装和配置 p29


### 2.3 安装目录分析
- bin 包含了mvn运行的脚本       
- boot 类加载器框架
- conf setting.xml  全局定制Maven的行为
- lib Maven运行时需要的Java类库

### 2.4 设置 HTTP代理

### 3.1  编写 POM       p48
>groupId、artifactId和version       

- groupId定义了项目属于哪个组，如果XXX建立一个名为 myapp的项目，那么groupId就是 com.XXX.myapp
- artifactId定义了当前Maven项目在组中唯一的ID
- version定义了 项目的版本
- name元素声明了一个对于用户更为友好的项目名称

### 3.2 编写主代码
项目的主代码会被打包到最终的构建中（如 jar），而测试代码只在运行测试时用到     
Maven项目中默认的主代码目录是 src/main/java

代码编写完毕后，使用Maven进行编译，在项目根目录下运行命令 
    
    mvn clean compile
    
clean告诉Maven清理输出目录 target/, compile告诉编译项目主代码

### 3.3 编写测试代码
Maven项目中默认的测试代码目录是 src/test/java       

>在POM.xml中为项目添加依赖包        p50

在POM.xml中添加 dependencies元素，该元素下可以包含多个dependency元素以声明项目的依赖      
- 在POM中一个值为test的元素 scope，scope为依赖范围，test表示该依赖只对测试有效；默认的依赖范围值是 compile，表示该依赖对主代码和测试代码都有效

代码编写完后 调用Maven进行测试 

    运行 mvn clean test
    
### 3.4 打包和运行

    mvn clean package
    
为了生成可执行的jar文件，需要借助 maven-shade-plugin插件

### 3.5 使用 Archetype生成项目骨架
Maven项目中对于项目结构的约定称为项目骨架，Maven3可以使用 maven archetype来创建项目的骨架

    mvn archetype:generate
有很多可用的Archetype供选择，默认选择 Archetype，接着输入 要创建项目的 groupId、artifactId、version以及包名 package

可以开发自己的 Archetype来快速生成项目骨架

### 3.6 在IDE中使用 Maven

#### 3.6.2 创建Maven项目

## 第四章 背景案例

### 4.4 小节  p69