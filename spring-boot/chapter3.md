## Spring Boot 是什么

众所周知 Spring 应用需要进行大量的配置，各种 XML 配置和注解配置让人眼花撩乱，且极易出错，因此 Spring 一度被称为配置地狱

为了简化 Spring 应用的搭建和开发过程，Pivotal 团队在 Spring 基础上提供了一套全新的开源的框架，就是 Spring Boot

Spring Boot 具有 Spring 一切优秀的特性，而且使用更加简单，功能更加丰富，性能更加健壮，随着近些年
微服务技术的流行，Spring Boot 成为炙手可热的技术

Spring Boot 提供了大量开箱即用的依赖模块，如 spring-boot-starter-redis, spring-boot-starter-data-mongodb, spring-boot-starter-data-elasticsearch 等。
这些依赖模块为 Spring Boot 应用提供了大量的自动配置，使得 Spring Boot 应用只需非常少量的配置
甚至是零配置，便可以运行起来，从而有更多的精力专注于业务逻辑开发

### Spring Boot 具有以下特点

- 独立运行的 Spring 项目，Spring Boot 可以以 jar 包的形式独立运行，只需要通过`java-jar xxx.jar`即可运行

- 嵌入 Servlet 容器，Spring Boot 使用嵌入式的 Servlet 容器（Tomcat, Jetty, Undertow 等）
  应用无需打包成 war 包

- 提供 starter 简化 Maven 配置，提供了一系列的`starter`项目对象模型(POMS)来简化 Maven 配置

- 提供了大量的自动配置

- 自带应用监控

- 无代码生成和 xml 配置

## 使用 Maven 创建一个 Spring Boot 项目

在 Maven 项目的 pom.xml 中添加 Spring Boot 相关的依赖

```xml
<project>

  <parent>
  	<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.6.4</version>
		<relativePath/> <!-- lookup parent from repository -->
  </parent>

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

在`com.a42work.yba` 包下，创建一个名为 helloWorldApplication 主程序，用来启动 Spring Boot 应用

```java
package com.a42work.yba;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class helloWorldApplication {
  public static void main(String[] args) {
    SpringApplication.run(helloWorldApplication.class, args);
  }
}

```

## starter

传统的 Spring 项目想要运行，不仅要导入各种依赖，还要对各种 xml 配置文件进行配置，十分繁琐，但 Spring Boot
不进行任何配置也能直接运行，这要归功于 Spring Boot 的 starter 机制

Spring Boot 将日常企业应用研发中的各种场景都抽取出来，做成一个个的 starter（启动器），starter 中整合了该
场景下各种可能用到的依赖，用户只需要在 Maven 中引入 starter 依赖，Spring Boot 就能自动扫描到要加载并启动
相应的默认配置

starter 提供了大量的自动配置，让用户摆脱了处理各种依赖和配置的困扰，所有这些 starter 都遵循着约定俗成的默认配置，
并允许用户调整这些配置

以 spring-boot-starter-web 为例，它能够为提供 web 开发场景所需要的几乎所有依赖，因此在使用 Spring Boot
开发 web 项目时，只需要引入该 starter 即可，而不需要额外导入 web 服务器和其他的 web 依赖

在项目中 mvn 命令查看依赖树

```
mvn dependency:tree
```

Spring Boot 导入了 springframework, logging, jackson, Tomcat 等，这些都是开发 web 项目所需要的

您可能会发现，在 pom.xml 配置中，引入 spring-boot-starter-web 时并没有指明其版本，但在依赖树中，我们
却看到所有的依赖都具有版本信息。这些版本信息是由 spring-boot-starter-parent 统一控制的

## spring-boot-starter-parent

spring-boot-starter-parent 是所有 Spring Boot 项目的父级依赖，被称为 Spring Boot 的版本仲裁中心，
可以对项目内的部分常用依赖进行统一管理

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.6.4</version>
  <relativePath/> <!-- lookup parent from repository -->
</parent>
```

Spring Boot 项目可以通过继承 spring-boot-starter-parent 来获得一些合理的默认配置

- 默认 JDK 版本（Java8）

- 默认字符集（UTF-8）

- 依赖管理功能

- 资源过滤

- 默认插件配置

- 识别`applicaton.properties`和`application.yml`类型的配置文件

spring-boot-dependencies 的底层代码

- dependencyManagement 负责管理依赖

- pluginManagement 负责管理插件

- properties 负责定义依赖或插件的版本号

spring-boot-dependencies 通过 dependencyManagement、pluginManagement、properties 等元素
对一些常用技术框架的依赖或插件进行统一版本管理，Activemq, Spring, Tomcat 等

## YAML

Spring Boot 提供了大量的自动化配置，极大的简化了 spring 应用的开发过程，用户也可以根据自身的需要
使用配置文件修改 Spring Boot 的默认设置

Spring Boot 默认使用 2 种全局的配置文件，其文件名是固定的

- application.properties

- application.yml

application.yml 是一种使用 YAML 语言编写的文件，它与 application.properties 一样，可以在
Spring Boot 启动时被自动读取

### YAML 简介

YAML 全称 Ain't Markup Language，它是一种数据为中心的标记语言

下面是一个简单的 application.yml 属性配置文件

```yml
server:
  prot: 8081
```

### YAML 语法

- 使用缩进表示层级关系

- 缩进时不允许使用 Tab 键，只允许使用空格

- 缩进的空格数不重要，但同级元素必须左对齐

- 大小写敏感

### YAML 常用写法

- 对象，键值对的集合

- 数组，一组按次序排序的值

- 字面量，单个的、不可拆分的值

### YAML 字面量写法

字面量是指单个的、不可拆分的值，如数字、字符串、布尔值、日期等

在 YAML 中，使用`key:[空格]value`的形式表示一对键值对，空格不能省略

字面量直接写在 value 的位置即可，且默认情况下字符串是不需要使用单引号或双引号的

**若使用单引号，会转译特殊字符**

```yml
name: 'zhangsan \n lisi'
```

输出结果: `zhangsan \n lisi`

**若使用双引号，则不会转译特殊字符**

```yml
name: "zhangsan \n lisi"
```

输出结果:

```
zhangsan
lisi
```

**YAML 为对象提供了 2 种写法**

普通写法，使用缩进表示对象与属性的层级关系

```yml
website:
  name: bianchengbang
  url: www.biancheng.net
```

行内写法

```yml
website: { name: bianchengbang, url: www.biancheng.net }
```

### YAML 数组写法

```yml
pets:
  - dog
  - cat
  - pig
```

行内写法

```yml
pets: [dog, cat, pig]
```

### YAML 组织结构

一个 YAML 文件可以由一个或多个文档组成，文档之间使用`---`作为分隔符，且文档相互独立，互补干扰。如果 YAML
只包含一个文档，则`---`分隔符可以省略

```yml
---
website:
  name: bianchengbang
  url: www.biancheng.net
---
pets:
  - dog
  - cat
  - pig
```
