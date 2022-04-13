# Spring Boot 配置绑定

所谓配置绑定就是把配置文件中的值与 JavaBean 中对应的属性进行绑定，通常会把一些配置信息，如数据库配置
放在配置文件中，然后通过 Java 代码去读取该配置文件，并把配置文件中指定的配置封装到 JavaBean 实体类中

Spring Boot 提供了 2 种方式进行配置绑定

- 使用 @ConfigurationProperties 注解

- 使用 @Value 注解

演示如何通过`@ConfigurationProperties`注解进行配置绑定

在 helloworld 的全局配置文件 application.yml 中添加自定义属性

```yml
person:
  lastName: 张三
  age: 18
  birth: 1990/12/21
  dog:
    name: 迪迪
    age: 3
```

在`net.biancheng.www.bean`中创建一个名为 Person 的实体类，并将配置文件中的属性映射到这个实体类上

```java
package net.biancheng.www.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

/*
 * @ConfigurationProperties 告诉Spring Boot 将本类中的所有属性和配置文件中
 * 相关的配置进行绑定
 * prefix = "Person" 配置文件中的哪个下面的属性进行一一映射
 *
 * 只有这个组件是容器中的组件，才能使用容器提供的@ConfigurationProperties功能
 */

@Component
@ConfigurationProperties(prefix = "Person")
public class Person {
  private String lastName;
  private Interger age;
  private Date birth;
  private Dog dog;

  public Person() {}
}
```

## @Value

当我们只需要读取配置文件中的某一个配置时，可以通过@Value 注解获取

```java
package net.biancheng.www.bean;

@Component
public class Person {
  @Value("${Person.lastName}")
  private String lastName;
  @Value("${Person.age}")
  private Integer age;
}
```

## @ConfigurationProperties 与 @Value 对比

@ConfigurationProperties 支持所有类型的数据封装，例如 Map, List, Set 以及对象等

@Value 只支持基本数据类型的封装，如字符串，布尔值，整数等

## @PropertySource

如果将所有的配置都集中到 application.properties 或 application.yml 中，那么这个配置文件会十分的臃肿
且难以维护，因此我们通常会将与 Spring Boot 无关的配置，例如自定义配置提取出来，写在一个单独的配置文件中，并在
对应的 JavaBean 上使用`@PropertySource`注解指向该配置文件

person.properties 的配置如下

```
person.last-name=李四
person.age=12
person.birth=1990/12/15
```

在 Person 使用@PropertySource 注解指向 person.properties

```java
package net.biancheng.www.bean;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.PropertySource;
import org.springframework.stereotype.Component;

@PropertySource(value = "classpath:person.properties") // 指向配置文件
@Component
@ConfigurationProperties(prefix = "person")
public class Person {}
```

# Spring Boot 导入 Spring 配置

默认情况下，Spring Boot 中是不包含任何的 Spring 配置文件的，即使我们手动添加 Spring 配置文件到项目中，
也不会被识别

Spring Boot 为我们提供了 2 种方式来导入 Spring 配置

- 使用@ImportResource 注解加载 Spring 配置文件

- 使用全注解方式加载 Spring 配置

## @ImportResource 导入 Spring 配置文件

在主启动类上使用@ImportResource 注解可以导入一个或多个 Spring 配置文件，并使其中的内容生效

在 net.biancheng.www.service 包下创建一个名为 PersonService 的接口

```java
package net.biancheng.www.service;

import net.biancheng.www.bean.Person;

public interface PersonSerivce {
  public Person getPersonInfo();
}
```

在 net.biancheng.www.service.impl包下创建一个名为PersonServiceImpl 的类，并实现 PersonService 接口

```java
package net.biancheng.www.service.impl;

import net.biancheng.www.bean.Person;
import net.biancheng.www.service.PersonService;
import org.springframework.beans.factory.annotation.Autowired;

public class PersonServiceImpl implements PersonService {
  @Autowired
  private Person person;


  @Override
  public Person getPersonInfo() {
    return person
  }
}
```

在该项目的 resources 下添加一个名为 beans.xml 的 Spring 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="personService" class="net.biancheng.www.service.impl.PersonServiceImpl"></bean>
</beans>
```

在主启动程序类上使用@ImportResource 注解，将 Spring 配置文件 beans.xml 加载到项目中

```java
package net.biancheng.www;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.ImportResource;

// 将beans.xml 加载到项目中
@ImportResources(locations = {"classpath:/beans.xml"})
@SpringBootApplication
public class HelloworldApplication {

  public static void main(String[] args) {
    SpringApplication.run(HelloworldApplication.class, args);
  }
}
```

## 全注解方式加载 Spring 配置

Spring Boot 推荐我们使用全注解的方式加载 Spring 配置

- 使用@Configuration 注解定义配置类，替换 Spring 的配置文件

- 配置类内部可以包含一个或多个被@Bean 注解的方法，这些方法会被 AnnotationConfigApplicationContext 或
  AnnotationConfigWebApplicationContext 类扫描，构建 bean 定义(相当于 Spring 配置文件中的<bean></bean>标签)，
  方法的返回值会以组件的形式添加到容器中，组件的 id 就是方法名

```java
package net.biancheng.www.config;

import net.biancheng.www.service.PersonService;
import net.biancheng.www.service.impl.PersonServiceImpl;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * @Configuration 注解用于定义一个配置类，相当于Spring 的配置文件
 * 配置类中包含一个或多个被@Bean注解的方法，该方法相当于Spring 配置文件中的<bean>标签定义的组件
 */

@Configuration
public class MyAppConfig {
  /**
   * 与 <bean id="personService" class="PersonServiceImpl"></bean> 等价
   * 该方法返回值以组件的形式添加到容器中
   * 方法名是组件 id
   */
   @Bean
   public PersonService personService() {
     System.out.println("在容器中添加了一个组件: personService");
     return new PersonServiceImpl();
   }
}
```
