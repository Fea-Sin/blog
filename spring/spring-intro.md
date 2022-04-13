# Spring 入门

Spring 是一个轻量级 Java 开发框架，目的是为了解决企业级应用开发的业务逻辑层和其他各层的耦合问题
为开发 Java 应用程序提供全面的基础架构支持，Spring 负责基础架构，因此 Java 开发者可以专注于
应用的开发

Spring 框架至今已集成了 20 多个模块，这些模块分布在核心容器(Core Container)，数据访问/集成(Data Access/Integration)层，
web 层，AOP(Aspect Oriented Programming)面向切面的编程，植入(Instrumentation)，消息传输(Messaging)和测试模块中

## 核心容器

Spring 的核心容器是其他模块建立的基础，由 Spring-core、Spring-beans、Spring-context、Spring-context-support 和
Spring-expression(Spring 表达式语言)等模块组成

Spring-core 模块，提供了框架的基本组成部分，包括控制反转(Inversion of Control)，依赖注入(Dependency Inject)功能

Spring-beans 模块，提供了 BeanFactory，是工厂模式的一个典型实现，Spring 将管理对象称为 Bean

Spring-context 模块，建立在 Core 和 Beans 模块的基础之上，提供了一个框架式的对象访问方式，是访问定义和配置的任何对象的媒介

## AOP 和 Instrumentation

Spring-aop 模块，提供了一个 AOP 要求的面向切面的编程实现，允许定义方法拦截器和切入点，将代码按照功能进行分离，
以便干净地解耦

Spring-instrument 模块，提供了类植入支持和类加载器的实现，可以在特定的应用服务器中使用

## 数据访问/集成

数据访问/集成层由 JDBC、ORM、OXM、JMS 和事务模块组成

Spring-jdbc 模块，提供了一个 JDBC 的抽象层，消除了繁琐的 JDBC 编码和数据库厂商特有的错误代码解析

Spring-orm 模块，为流行的对象关系映射 API 提供集成层，包括 JPA 和 Hibernate

Spring-oxm 模块，提供了一个支持对象 XML 映射的抽象层实现

## Web

Web 层由 Spring-web、Spring-webmvc、Spring-websocket 和 Portlet 模块组成

Spring-web 模块，提供了基本的 Web 开发集成功能，例如多文件上传功能、使用 Serlet 监听器初始化一个 Ioc 容器以及 Web 应用上下文

Spring-webmvc 模块，也称为 Web-Servlet 模块，包含用于 Web 应用程序的 Spring MVC 和 REST Web Services 实现

## 测试

Spring-test 模块支持使用 JUnit 或 TestNG 对 Spring 组件进行单元测试和集成测试
