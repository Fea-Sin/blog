# Spring IoC

IoC(控制反转) 是 Spring 框架的基础，也是 Spring 框架的核心理念，用来消减计算机程序的耦合问题。

依赖注入(Dependency Injection DI)是 IoC 的另外一种说法

当某个 Java 对象需要调用另一个 Java 对象(被依赖对象)时，在传统编程模式下，调用者通常会采用`new`被调用
者的代码方式来创建对象。这种方式会增加调用者和被调用者之间的耦合性，不利于后期代码的升级和维护

当 Spring 框架出现时，对象的实例不再由调用者来创建，而是由 Spring 容器来创建，Spring 容器会负责控制程序之间
的关系，而不是由调用者的程序代码直接控制。这样控制权由调用者转移到 Spring 容器，控制权发生了反转，这就是 Spring
的控制反转

从 Spring 容器角度来看，Spring 容器负责将被依赖对象赋值给调用者的成员变量，相当于为调用者注入它所依赖
的实例，这就是 Spring 的依赖注入

控制反转是一种通用的描述（在 Spring 中可以是 XML 或注解）并通过第三方去产生或获取特定对象的方式。在
Spring 中实现控制反转的是 IoC 容器，其实现方式是依赖注入

Spring IoC 容器的设计主要是基于 BeanFactory 和 ApplicationContext 两个接口

`BeanFactory`由`org.springframework.beans.factory.BeanFactory`接口定义，它提供了
完整的 IoC 服务支持，是一个管理 Bean 的工厂，主要负责初始化 Bean。

`BeanFactory`接口有多个实现类，其中比较常用的是`org.springframework.beans.factory.xml.XmlBeanFactory`，
该类会根据 XML 配置文件中的定义来装配 Bean

`ApplicationContext` 是`BeanFactory`的子接口，也称为应用上下文，由`org.springframework.context.ApplicationContext`
接口定义

`ApplicationContext`接口除了包含`BeanFactory`的所有功能以外，还添加了对国际化、资源访问、事件传播等内容的支持

## 依赖注入的类型

在 Spring 中实现 IoC 容器的地方是依赖注入，依赖注入的作用是在使用 Spring 框架创建对象时动态的将其所依赖的对象注入
`Bean`组件中

Spring 框架的依赖注入通常有两种实现方式，一种是使用构造方法注入，另一种是使用属性的 setter 方法注入

Spring 框架可以采用 Java 的反射机制，通过构造方法完成依赖注入

使用 setter 方法注入是 Spring 框架中最主流的注入方法，它利用 Java Bean 规范所定义的 setter 方法来完成注入，
灵活且可读性高。对于 setter 方法注入，Spring 框架也是使用 Java 的反射机制实现的
