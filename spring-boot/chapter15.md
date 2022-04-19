# Spring Boot JDBC 访问数据库

对于数据访问层，无论是 SQL(关系数据库)还是 NoSQL(非关系数据库)，Spring Boot 都默认采用整合 Spring Data
的方式进行统一处理，我们只需要进行简单的设置即可实现对数据的访问

## 导入 JDBC 场景启动器

Spring Boot 将日常企业应用研发中的各种场景都抽取出来，做成一个个的场景启动器(starter)，场景启动器中整合了
该场景下各种可能用到的依赖

想要在 Spring Boot 中使用 JDBC 进行数据访问，第一步就是要在 pom.xml 中导入 JDBC 场景启动器 `spring-boot-starter-data-jdbc`

```xml
<!-- 导入JDBC的场景启动器 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>
```

导入数据库驱动

JDBC 的场景启动器中并没有导入数据库驱动，我们需要根据自身的需求引入所需的数据库驱动，例如，访问 MySQL 数据库时，
需要导入 MySQL 的数据库驱动`mysql-connector-java`

```xml
<!-- 导入数据库驱动 -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <scope>runtime</scope>
</dependency>
```

> Spring Boot 默认为数据库驱动程序做了版本仲裁，所以我们在导入数据库驱动时，可以不再声明版本。需要注意的是，数据
> 库驱动的版本必须与数据库的版本相对应

## 配置数据源

在配置文件`application.properties` 中配置数据源

```yml
# 数据源连接信息
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://127.0.0.1:3306/bianchengbang_jdbc
    driver-class-name: com.mysql.cj.jdbc.Driver
```

## 测试

Spring Boot 提供了一个名为 JdbcTemplate 的轻量级数据访问工具，它是对 JDBC 的封装。Spring Boot
对 JdbcTemplate 提供了默认自动配置，我们可以直接使用@Autowired 或构造函数将它注入到 bean 中

```java
package net.biancheng.www;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.jdbc.core.JdbcTemplate;

import javax.sql.DataSource;
import javax.sql.SQLException;

@SpringBootTest
class SpringBootJdbcApplicationTests {
  // 数据源组件
  @Autowired
  DataSource dataSource;

  // 用于访问数据库的组件
  @Autowired
  JdbcTemplate jdbcTemplate;

  @Test
  void connextLoads() throws SQLException {
    System.out.println("默认数据源：" + dataSource.getClass());
    System.out.println("数据库链接实例：" + dataSource.getConection());

    // 访问数据库
    Integer i = jdbcTemplate.queryForObject("SELECT count(*) from `user`", Integer.class);
     System.out.println("user表中共有：" + i + "条数据");
  }
}
```

# Spring Boot 数据源配置原理

在数据库访问过程中，数据源无疑是最重要的概念之一，它不仅可以对数据库访问相关的各种参数进行封装和统一管理，
还可以管理数据库连接池，提高数据库连接性能

市面上优秀的开源数据源，例如 DBCP、C3P0、Druid、HikariCP 等等

## DataSourceAutoConfiguration

Spring Boot 中几乎所有的默认配置都是通过配置类 `XxxAutoConfiguration` 进行配置，数据源也不例外，它的自动配置
类是 DataSourceAutoConfiguration

在用户没有配置数据源的情况下，若容器中存在 HikaiDataSource 类，则 Spring Boot 就会实现自动化实例 Hikai，并将
其作为数据源

Spring Boot de JDBC 场景启动器，通过 spring-boot-starter-jdbc 默认引入 HikaiCP 数据源
