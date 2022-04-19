# Spring Boot 整合 Druid 数据源

Spring Boot 2.x 默认使用 HikaiCP 作为数据源，可以使用 HikaiCP 数据源获取数据库连接，对数据库
进行增删改查等操作

Druid 是目前国内使用范围最广的数据源产品

Druid 是阿里巴巴推出的一款开源的高性能数据源产品，Druid 支持所有 JDBC 兼容的数据库，包括 Oracle, MySQL, SQL Server
和 H2 等等。Druid 不仅结合了 C3P0, DBCP, PROXOOL 等数据源产品的优点，同时还加入了强大的监控功能

通过 Druid 的监控功能，可以实时观察数据库连接池和 SQL 的运行情况，帮助用户及时排查出系统中的问题

Druid 不是 Spring Boot 内部提供的技术，它属于第三方技术，我们可以通过以下两种方式进行整合

- 自定义整合 Druid
- 通过 starter 整合 Druid

## 引入 Druid 依赖

Druid 0.1.18 之后版本都已经发布到 Maven 中央仓库中了，所以我们只需要在项目的 pom.xml 中引入 Druid 的依赖
即可将 Druid 导入到 Spring Boot 项目中

```xml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid</artifactId>
  <version>1.2.6</version>
</dependency>
```

在 net.biancheng.www.config 包中，创建一个名为 MyDataSourceConfig 的配置类，并将 Druid 数据源对象
添加到容器中

```java
package net.biancheng.www.config;

import com.alibaba.druid.pool.DruidDataSource;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import javax.sql.DataSource;
import java.sql.SQLException;

@Configuration
public class MyDataSourceConfig implements WebMvcConfigurer {
  /**
   * 向容器中添加 Druid 数据源
   * 使用 @ConfigurationProperties 将配置文件中 spring.datasource 开头的配置与数据中的属性
   * 进行绑定
   */
   @ConfigurationProperties("spring.datasource")
   @Bean
   public DataSource dataSource() throws SQLException {
     DruidDataSource druidDataSource = new DruidDataSource();

     return druidDataSource;
   }
}
```

在配置文件 application.yml 中添加数据源配置，它们将会与 Druid 数据源中的属性进行绑定

```yml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://127.0.0.1:3306/bianchengbang_jdbc
    driver-class-name: com.mysql.cj.jdbc.Driver
```

## 开启 Druid 内置监控页面

Druid 内置提供了一个名为 StatViewServlet 的 Servlet，这个 Servlet 可以开启 Druid 的内置监控页面功能，
展示 Druid 的统计信息

想要开启 Druid 的内置监控页面，需要将该 Servlet 配置在 Web 应用中的 WEB_INF/web.xml 中，web.xml 配置

```xml
<servlet>
  <servlet-name>DruidStatView</servlet-name>
  <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
</servlet>
<servlet-mapping>
  <servlet-name>DruidStatView</servlet-name>
  <url-pattern>/druid/*</url-pattern>
</servlet-mapping>
```

Spring Boot 项目中是没有 WEB-INF/web.xml 的，我们可以在配置类中，通过 ServletRegistrationBean 将 StatViewServlet
注册到容器中，来开启 Druid 的内置页面

在 MyDataSourceConfig 配置类中添加如下代码，将 StatViewServlet 注入到容器中，开启 Druid 内置监控页面功能

```java



@Bean
public ServletRegistrationBean statViewServlet() {
  StatViewServlet statViewServlet = new StatViewServlet();

  ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(statViewServlet, "/druid/*")
  servletRegistrationBean.addInitParameter("loginUsername", "admin");
  servletRegistrationBean.addInitParameter("loginPassword", "123456");

  return servletRegistrationBean;
}
```

## 通过 starter 整合 Druid

Spring Boot 官方只提供了对 Hikai, Tomcat, Dbcp2, OracleUcp 等 4 种数据源自动配置支持

阿里官方提供了 Druid Spring Boot Starter，它可以帮助我们在 Spring Boot 项目中，轻松地整合 Druid 的数据库
连接池和监控功能

### 引入依赖

在 Spring Boot 项目中的 pom.xml 中添加 Druid Spring Boot Starter

```xml
<dependency>
  <groupId>com.alibaba</groupId>
  <artifactId>druid-spring-boot-starter</artifactId>
  <version>1.1.17</version>
</dependency>
```

### 配置属性

- JDBC 通用配置

- Druid 数据连接池配置

- Druid 监控配置

- Druid 内置 Filter 配置

**JDBC 通用配置**

```yml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql://127.0.0.1:3306/bianchengbang_jdbc
    driver-class-name: com.mysql.cj.jdbc.Driver # 数据驱动
```

**Druid 数据源连接配置**

```yml
spring:
  datasource:
    druid:
      initital-size: 5 # 初始化连接大小
      min-idle: 5 # 最小连接池数量
      max-active: 20 # 最大连接池数量
      max-wait: 6000 # 获取连接时最大等待时间，单位毫秒
```

**Druid 监控配置**

```yml
spring:
  datasource:
    druid:
      start-view-servlet:
        enabled: true # 是否开启内置监控页面，默认值是false
        url-pattern: "/druid/*"
        reset-enable: true
        login-username: admin
        login-password: admin
      web-start-filter:
        enable: true
        url-pattern: "/*"
        exclusions: "*.js, *.gif, *.jpg, *.png, *.css, *.ico, /druid/*"
```
