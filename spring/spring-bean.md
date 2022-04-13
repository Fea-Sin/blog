# Spring Bean

Spring Bean 的配置、实例化、作用域、生命周期以及装配方式

在 Spring 的应用中，Spring IoC 容器可以创建、装配和配置应用组件对象，这里的组件对象称为 Bean，
本节重点介绍如何将 Bean 装配到 Spring IoC 容器中

Spring 可以看作一个大型工厂，用于生产和管理 Spring 容器中的 Bean。如果要使用这个工厂生产和管理 Bean，
需要开发者将 Bean 配置在 Spring 的配置文件中。Spring 框架支持`XML`和`Properties`两种格式的配置文件

XML 配置文件的根元素是`<beans>`，`<beans>`中包含了多个`<bean>`子元素，每个`<bean>`元素定义一个 Bean，
并 Bean 描述如何被装配到 Spring 容器中

### <bean>元素的常用属性及子元素

- `id` Bean 在 BeanFactory 中唯一的标识，在代码中通过 BeanFactory 获取 Bean 实例时需要以此作为索引名称

- `class` Bean 的具体实现类，使用的类的名

## Bean 的实例化

在面相对象编程中，如果想使用某个对象，需要事先实例化该对象。同样，在 Spring 框架中，如果想使用
Spring 容器中的 Bean，也需要示例化 Bean。
Spring 框架实例化 Bean 有 3 种方式，即构造方法实例化、静态工厂实例化和实例工厂实例化

在 Spring 框架中，Spring 容器可以调用 Bean 对应类中的无参数构造函数来实例化 Bean，这种方式称为
构造方法实例化

### 静态工厂实例化

在使用静态工厂实例化 Bean 时要求开发者在工厂类中创建一个静态方法来创建 Bean 的实例。在配置 Bean 时，
clss 属性指定静态工厂类，同时还需要使用`factory-method`属性指定工厂类中的静态方法

### 示例工厂实例化

在使用实例工厂实例化 Bean 时要求开发者在工厂类中创建一个实例方法来创建 Bean 的实例。
在配置 Bean 时需要使用`factory-bean`属性指定配置的实例工厂，同时还需要使用`factory-method`
属性指定实例工厂中的实例方法

## Bean 的作用域

在 Spring 中不仅可以完成 Bean 的实例化，还可以为 Bean 指定作用域

- `singleton` 默认作用域，使用 singleton 定义在 Bean 在 Spring 容器中只有一个 Bean 实例

- `prototype` Spring 容器每次获取 prototype 定义的 Bean，容器都将创建一个新的 Bean 实例

### singleton 作用域

当将 bean 的 scope 设置为 singleton 时，Spring IoC 容器仅生成和管理一个 Bean 实例。在使用
id 或 name 获取 Bean 实例时，IoC 容器将返回共享的 Bean 实例

### prototype 作用域

将 bean 的 scope 设置为 prototype 时，Spring IoC 容器将为每次请求创建一个新的实例

使用 id 或 name 两次获取 Bean 实例时，IoC 容器将返回两个不同的 Bean 实例

## Bean 的生命周期

Spring 容器可以管理 singleton 作用域 Bean 的生命周期，在此作用域下，Spring 能够精确地知道 Bean
何时被创建，何时初始化完成，以及何时被销毁。
而对于 prototype 作用域的 Bean，Spring 只负责创建，当容器创建了 Bean 实例后，Bean 实例就交给了客户端的
代码管理，Spring 容器将不再跟踪其生命周期

Spring 中 Bean 的生命周期的执行是一个很复杂的过程，可以借鉴 Servlet 的生命周期

```
实例化-> 初始化 -> 接收请求 -> 销毁
```

## Bean 的装配方式

Bean 的装配可以理解为将 Bean 依赖注入到 Spring 容器中，Bean 的装配方式即 Bean 依赖注入的方式。
Spring 容器支持基于 XML 配置的装配、基于注解的装配以及自动装配等多种装配方式

最受欢迎的装配方式是基于注解的装配

### 基于 XML 配置的装配

基于 XML 配置的装配方式已经有很久的历史了，我们知道 Spring 提供了两种基于 XML 配置的装配方式，即使用构造
方法注入和使用属性的 setter 方式注入

### 基于注解的装配

在 Spring 框架中，尽管使用 XML 配置文件可以很简单地装配 Bean，但如果应用中有大量的 Bean 需要装配，
会导致 XML 配置文件过于庞大，不方便以后的升级与维护

在 Spring 框架中定义了一系列的注解

@Repository 标注数据访问层
@Service 标注业务逻辑层
@Controller 标注控制器层
