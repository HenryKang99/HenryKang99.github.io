---
title: Maven 入门笔记
toc: true
share: true
donate: true
comment: true
tags: [后端]
categories: [JavaEE,Back End]
date: 2020-03-22
description:
top_img: false
cover: https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/prG8HpKRhh93.jpg
---

### 0verview

> **官方介绍**
>
> Maven, a [Yiddish word](https://en.wikipedia.org/wiki/Maven) meaning *accumulator of knowledge*, began as an attempt to simplify the build processes in the Jakarta Turbine project. There were several projects, each with their own Ant build files, that were all slightly different. JARs were checked into CVS. We wanted **a standard way to build the projects, a clear definition of what the project consisted of, an easy way to publish project information and a way to share JARs across several projects.**
>
> Maven 是一个项目管理工具, 帮助我们 清理、编译、测试、打包、发布项目……



<!-- more -->

### 1. Hello Maven

#### 1.1 安装maven

1. 官网下载,  解压到没有中文和空格的目录下即可;
2. 配置 MAVEN_HOME 与 PATH 环境变量;
3. 要正确配置JAVA_HOME；
   - 若使用的Maven版本较新的话，要去官网查看对应的版本所需最低的JDK要求;
4. cmd 输入 `mvn -v` 检验是否安装成功;

#### 1.2 配置本地仓库 & 中央仓库

> 核心配置文件`/conf/settings.xml`;
> 本地仓库没有找到所需资源时, 会去中央仓库下载, 而国外镜像下载较慢, 所以我们切换到aliyun镜像;

1. 网上搜索下载一个较全面的maven本地仓库;
2. 在`settings.xml`中配置标签`<localRepository>`为本地仓库路径;

```xml
<localRepository>D:\DevelopmentKit\maven_repository</localRepository>
```

3. 继续在`<mirrors>`标签体中配置如下`<mirror>`

```xml
<mirror>
	<id>alimaven</id>
	<mirrorOf>central</mirrorOf>
	<name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>       
</mirror>
```

- **注意:** `/conf`下的setting称为全局配置文件, 在windows用户目录下`.m2`下 还可以声明一个用户配置文件, 优先使用用户配置文件. 如果修改了全局配置文件, 死活都没有生效, 就要考虑是不是有用户配置文件. 使用IDE创建maven工程时可以选择配置信息. 

#### 1.3 Maven常用命令

> 先只记一些笼统的命令, 便于理解maven的生命周期, 还有很多具体的命令细节参数等, 以后用到再做补充.

 ![](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/20200215095017.png)

| 命令         | 描述                                                 |
| :----------- | :--------------------------------------------------- |
| mvn  clean   | 清理之前编译出的文件（编译的文件放在tagart文件夹中） |
| mvn  compile | 编译除了test                                         |
| mvn  test    | 编译所有包括test                                     |
| mvn  package | 编译所有且打包                                       |
| mvn  install | 编译所有并打包且安装到本地仓库                       |
| mvn  deploy  | 部署到远程仓库                                       |

#### 1.4 IDEA创建Maven工程

1. IDEA设置中配置Maven根目录与配置文件位置等信息;

2. 新建一个maven工程, 选择使用 archetype 为 webapp;

3. `pom.xml`中添加依赖:

   - 依赖范围说明

   | scope         | 编译 | 运行 | 测试 | 例子         |
   | :------------ | :--: | :--: | :--: | ------------ |
   | compile(默认) |  √   |  √   |  √   |              |
   | test          |      |      |  √   | junit        |
   | provided      |  √   |      |  √   | jsp、servlet |
   | runtime       |      |  √   |  √   | jdbc         |
   | system        |  √   |      |  √   |              |

   - system：类似于 provided，区别在于声明为system则不会去仓库找jar包，而是需要显式的提供jar包；

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>servlet-api</artifactId>
    <version>2.5</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.0</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

4. `pom.xml`的`<build><plugins>`中配置maven编译插件和tomcat插件；

  - 为什么要配置这两个插件?
    - 因为maven默认的编译版本不符合我们的要求；
    - 可以使用idea的live template配置；
  - 配置tomcat插件成功后, 可以在plugins下看到 tomcat 的相关命令, 点击tomcat:run 即可运行;

```xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-compiler-plugin</artifactId>
	<version>3.1</version >
	<configuration>
		<source>1.8</source>
		<target>1.8</target>
		<encoding>UTF-8</encoding>
	</configuration>
</plugin>

<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
        <port>8080</port>
        <path>/</path>
    </configuration>
</plugin>
```

- 在`setting.xml`下`<profiles>`标签内可以全局设置编译版本, 这样就不用每次在pom中配置编译插件了,  如下:

```xml
<profile>
	<id>jdk-1.8</id>
	<activation>
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk>
	</activation>
    <properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	</properties>
</profile>
```

#### 1.4 Maven 项目目录结构

- 普通 Java 工程

```
|-src
|  --main
|    ---java
|    ---resources
|  --test
|    ---java
|    ---resources
|-target
|-pom.xml
```

- Java Web 工程

```
|-src
|  --main
|    ---java
|    ---resources
|    ---webapp
|      ----WEB-INF
|        -----web.xml web项目核心配置文件
|        -----classes 放置src编译后的字节码
|        -----lib	  放置依赖的jar包
|  --test
|    ---java
|    ---resources
|-pom.xml
```

　　maven创建的web工程**资源路径**写法要注意：不写`/`会在 resources 下寻找，写`/`会在classes中寻找。

