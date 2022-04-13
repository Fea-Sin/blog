# Spring Boot 自动配置原理

Spring Boot 项目创建完成后，即使不进行任何的配置，也能顺利进行，这要归功于 Spring Boot 的自动化配置

Spring Boot 默认使用 application.properties 或 application.yml 作为其全局配置文件，我们可以在该配置文件
中对各种自动配置属性 server.port、logging.level、spring.config.active.no-profile 等等

Spring Boot 中的配置属性数量庞大，我们只有了解 Spring Boot 自动配置原理，才能更加轻松熟练的对 Spring Boot 进行
配置

## Spring Factories 机制

Spring Boot 的自动配置是基于 Spring Factories 机制实现的。Spring Factories 机制是一种服务发现机制，
Spring Boot 会自动扫描所有 Jar 包类路径下`META-INF/spring.factories`文件，并读取其中的内容，进行实例化，
这种机制也是 Spring Boot Starter 的基础

Spring Factories 实现原理

spring-core 包里定义了 SpringFactoriesLoader 类，这个类会扫描所有 Jar 包类路径下的 META/spring.factories 文件，
并获取指定接口的配置

SpringFactoriesLoader 类中定义了两个对外的方法

loadFactories(Class<T> factoryType, @Nullable ClassLoader classLoader), 返回值`<T>List<T>`
: 静态方法，根据接口获取实现类的实例，该方法返回的是实现类对象列表

loadFactoryNames(Class<?> factoryType, @Nullable ClassLoader classLoader), 返回值`List<String>`
: 公共静态方法，根据接口获取实现类的名称，该方法返回的是实现类的类名的列表

## @SpringBootApplication 注解

所有 Spring Boot 项目主启动程序类上都使用了一个@SpringBootApplication 注解，该注解是 Spring Boot 中最重要的注解之一，
也是 Spring Boot 实现自动化配置的关键

@SpringBootApplication 是一个组合元注解，其主要包含两个注解：@SpringBootConfiguration 和@EnableAutoConfiguration

## @EnableAutoConfiguration 注解

@EnableAutoConfiguration 注解用于开启 Spring Boot 的自动配置功能，它使用 Sprign 框架提供的@Import 注解通过
AutoConfigurationImportSelector 类选择器给容器中导入自动配置组件

自动配置类 XxxAutoConfiguration 负责使用 XxxProperties 中属性进行自动配置，而 XxxProperties 则负责将自动属性与
配置文件的相关配置进行绑定，以便于用户通过配置文件修改默认的自动配置。也就是说，真正限制我们可以在配置文件中配置那些属性
的类就是这些 XxxProperties 类，它与配置文件中定义的 prefix 关键字开头的一组属性是唯一对应的
