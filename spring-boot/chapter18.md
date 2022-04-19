# Spring Boot 自定义 starter

starter 是 Spring Boot 中一种非常重要的机制，它可以将繁杂的配置统一集成到 starter 中，我们只需要
通过 maven 将 starter 依赖引入到项目中，Spring Boot 能自动扫描并加载相应的默认配置。

starter 的出现让开发人员从繁琐的框架配置中解放出来，将更多的精力专注于业务逻辑的开发，极大的提高了开发效率

我们也可以将一些通用功能封装成自定义的 starter 进行使用

## 命令规范

Spring Boot 提供的 starter 以 spring-boot-starter-xxx 的形式命令，官方建议第三方开发者或技术厂商自定义
的 starter 使用 xxx-spring-boot-starter 的形式命名，例如 mybatis-spring-boot-starter

## 自定义 starter

自定义 starter 可以分为 7 步

- 创建工程

- 添加 POM 依赖

- 定义 propertie 类

- 定义 Service 类

- 定义配置类

- 创建 spring.factories 文件

- 构建 starter

对项目进行构建

```
mvn clean install
```

当引用自定义 starter 的项目不在本地时，我们需要将自定义 starter 部署到远程仓库

```
mvn clean deploy
```

## 定义配置类

- @Configuration 表示该类是一个配置类

- @EnableConfigurationProperties(HelloProperties.class) 该注解的作用是为 HelloPropterties 开启属性配置性
  功能，并将这个类以组件的形式注入到容器中

- @ConditionalOnMissingBean(HelloService.class) 该注解表示当容器中没有 HelloService 类时，该方法才生效

- @Bean 该注解用于将方法的返回值以 Bean 对象的形式添加到容器中
