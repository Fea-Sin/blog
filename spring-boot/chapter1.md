# Maven

[地址](https://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/?self)

为什么使用 Maven 这样的构建工具

**一个项目就是一个工程**
如果项目非常大，就不适合使用 package 来划分模块，最好是每一个模块对应一个工程，
利于分工协作，借助于 Maven 就可以将一个项目拆分成多个工程

**项目中使用 jar 包，需要复制、粘贴到项目的 lib 中**

同样的 jar 包重复的出现在不同的项目工程中，你需要做不停的复制粘贴工作。借助于 Maven 可以将 jar
包保存在仓库中，不管在哪个项目只需要引用即可

**jar 包版本不一致的风险**

不同的项目在使用 jar 包的时候，有可能会导致各个项目的 jar 包版本不一致，maven 将所有
jar 包都放在仓库中，所有的项目都使用仓库中的一份 jar 包

**一个 jar 包依赖其他的 jar 包需要自己手动的引入到项目中**

极大的浪费了我们导入包的时间成本，借助于 Maven 它会自动将依赖的 jar 包导入进来

Maven 是一款服务于 java 平台的自动化构建工具

**构建**

把动态 Web 工程进过编译得到的编译结果部署到服务器上的整个过程，编译:java 源文件`.java`
编译成字节码文件`.class`，部署：在 sevlet 容器中部署编译后的文件

## 构建的各个环节

- 清理，将以前编译得到的旧文件 class 字节码文件删除

- 编译，将 java 源程序编译成 class 字节码

- 测试 test，自动测试

- 报告 report，测试程序执行的结果

- 打包 package，动态 Web 工程打 War 包，java 工程打 jar 包

- 安装 install，将打包得到的文件复制到仓库中的指定位置

- 部署 deploy，将动态 Web 工程生成的 War 包复制到 Servlet 容器下

## 安装 maven

当前系统是否配置 JAVA_HOME 的环境变量

下载 maven，解压 maven 放在一个非中文无空格的路径下

配置 maven 的相关环境变量
在环境变量增加 M2_HOME，路径是 maven 解压后的根目录
在环境变量里的 path 中增加 maven/bin 的路径

```
mvn -v
```

## 第一个 maven

maven 工程必须按照约定的目录结构创建

```
目录名(工程名)
  |
  src 源码
     |
    main(存放主程序)
       |
      java(java 源码)
      resource(存放框架的配置文件)
    test(存放测试文件)
  pop.xml(maven 的核心配置文件)
```

## maven 常用命令

- mvn clean(清理)

- mvn compile(编译主程序)

- mvn test-compile(编译测试程序)

- mvn test(执行测试)

- mvn package(打包)

- mvn install(安装)

执行 maven 命令必须到 pop.xml 的目录中进行执行

**仓库默认位置**

```
/Users/[系统用户]/.m2/repository
```

## 仓库和坐标

**pom.xml**: Project Object Model 项目对象模型，它是 maven 的核心配置文件，所有的构建的配置都在这里设置

**坐标**

使用下面的三个向量在仓库中唯一的定位一个 maven 工程

```xml
<!-- 公司或组织域名倒叙 + 项目名 -->
<groupId>com.a42work</groupId>

<!-- 模块名 -->
<artifactId>demo</artifactId>

<!-- 版本 -->
<version>0.0.1-SNAPSHOT</version>
```

maven 工程坐标于仓库中路径的关系

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

  <!-- 本项目坐标 -->
  <groupId>com.a42work</groupId>
	<artifactId>demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>

  <name>demo</name>
  <description>Demo project for Spring Boot</description>

  <dependencies>
  	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

    <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
  </dependencies>
</project>
```

maven 坐标和仓库对应的映射关系：`[groupId][artifactId][version][artifactId]-[version].jar`

## 仓库

本地仓库，当前电脑上的仓库

**远程仓库**

私服，搭建在局域网中，一般公司都会有私服，私服一般使用 nexus 来搭建

中央仓库，架设在 Internet 上，springframework 就是在中央仓库上

## 依赖

maven 解析依赖信息时会到本地仓库中去查找被依赖的 jar 包。对于本地仓库中没有的会去中央仓库去查找 maven 坐标
来获取 jar 包，获取到 jar 之后会下载到本地仓库。对于中央仓库也找不到依赖的 jar 包的时候，就会编译失败

如果依赖的是自己或者团队开发的 maven 工程，需要先使用 install 命令把依赖的 maven 工程的 jar 包导入到本地仓库中

举例： 现在我们再创建第二个 maven 工程 HelloFriend，其中用到了第一个 Hello 工程里类的 sayHello(String name)
方法，我们在给 HelloFriend 项目使用`mvn compile` 命令进行编译的时候，会提示缺少依赖 Hello 的 jar 包

这时，我们到第一个 maven 工程中执行`mvn install`后，你再去看一下本地仓库，你会发现 Hello 项目的 jar 包。一旦本地
仓库有了依赖的 maven 工程的 jar 包后，你再到 HelloFriend 项目中使用`mvn compile`命令的时候，就可以编译成功

## 依赖范围

**scope 就是依赖范围**

- compile，默认值，适用于所有阶段(开发、测试、部署、运行)，本 jar 会一直存在所有阶段

- provided，只在开发、测试阶段使用，目的是不让 Servlet 容器和你本地仓库的 jar 包冲突

- runtime， 只在运行时使用，如 JDBC 驱动，使用运行和测试阶段

- test，只在测试时使用，用于编译和运行测试代码。不会随项目发布

- system，类似 provided 需要显示提供包含依赖的 jar，maven 不会在 repository 中查找它

## 生命周期

maven 有三套相互独立的生命周期

**Clean Lifecycle** 在进行真正的构建之前进行一些清理工作

pre-clean 执行一些需要在 clean 之前完成的工作

clean 移除所有的上一次构建生成的文件

post-clean 执行一些需要在 clean 之后立刻完成的工作

**Default Lifecycle** 构建的核心部分，编译、测试、打包、部署等

- validate

- generate-sources

- process-sources

- generate-resources

- process-resources 复制并处理资源文件，至目标目录，准备打包

- compile 编译项目的源代码

- process-classes

- generate-test-sources

- process-test-sources

- generate-test-resources

- process-test-resources 复制并处理资源文件，至目标测试目录

- test-compile 编译测试源代码

- process-test-classes

- test 使用合适的单元测试框架运行测试，这些测试代码不会被打包或部署

- prepare-package

- package 接受编译好的代码，打包成可发布的格式如 jar

- pre-integration-test

- integration-test

- post-integration-test

- verify

- install 将包安装至本地仓库，以让其他项目依赖

- deploy 将最终的包复制到远程的仓库，让其他开发人员与项目共享

**Site Lifecycle** 生成项目报告、站点、发布站点

pre-site 执行一些需要在生成站点文档之前完成的工作

site 生成项目的站点文档

post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备

site-deploy 将生成的站点文档部署到特定的服务器上

## maven 工程的依赖高级特性

依赖传递性

pom.xml 文件配置好依赖关系后，必须首先`mvn install`后，依赖的 jar 包才能使用

非 compile 范围的依赖是不能传递的

### 依赖版本的原则

路径最短者优先原则

路径相同声明优先原则

## build 配置

配置好 build 后，执行`mvn package`之后，在 maven 工程指定的 target 目录里 war/jar 包和文件将按照配置生成
