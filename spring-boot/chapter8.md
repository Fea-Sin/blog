# Spring Boot 统一日志框架

在项目开发中，日志十分的重要，不管是记录运行情况还是定位线上问题，都离不开对日志的分析，在 Java 领域里存在着多种日志框架，
如 JCL、SLF4J、Jboss-logging、jUL、log4j、log4j2、logback 等等

市面上常见的日志框架有很多，它们可以被分为两类，日志抽象层和日志实现

日志抽象侧，为 Java 日志访问提供一套标准和规范的 API 框架，其主要意义在于提供接口，JCL, SLF4j, jboss-logging

日志实现，Log4j, JUL, Log4j2, Logback

Spring Boot 选用 SLF4J + Logback 的组合来搭建日志系统

## 统一日志框架

我们在使用 Spring Boot 时，同样可能用到其他的框架，例如 Mybatis, Spring MVC, Hibernate 等等，这些框架的底层
都有自己的日志框架，此时我们也需要对日志框架进行统一

# Spring Boot 日志配置及输出

Spring Boot 默认使用 SLF4J+Logback 记录日志，并提供了默认配置，即使我们不进行任何额外配置，也可以使用 SLF4J+Logback
进行日志输出

常见的日志配置包括日志级别、日志输出的格式等内容

**常见的日志级别**

- trace，追踪，指明程序运行轨迹

- debug，调试，实际应用中一般将其作为最低级别，而 trace 则很少使用

- info，输出重要的信息，使用较多

- warn，警告，使用较多

- error，错误信息，使用较多

**输出格式**

- `%d{yyyy-MM-dd HH:mm:ss, SSS}` 日志生产时间，输出到毫米的时间

- `%-5level` 输出日志级别，-5 表示左对齐并且固定输出 5 个字符，如果不足在右边补 0

- `%logger或%c` logger 的名称

- `%thread 或%t` 输出当前的线程名称

- `%p` 日志输出格式

- `%message或%msg或%m` 日志内容，即 logger.info('message')

- `%n` 换行符

- `%class 或 %C` 输出 Java 类名

- `%file 或 %F` 输出文件名

- `%L` 输出错误行号

- `%method 或 %M` 输出方法名

- `%l` 输出语句所在的行数、包括类名、方法名、文件名、行数

- `hostName` 本地机器名

- `hostAddress` 本地 ip 地址

## 在 Spring Boot 中编写测试类

```java
package net.biancheng.www;

import org.junit.jupiter.api.Test;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class SpringbootLoggingApplicationTests {
  Logger logger = LoggerFactory.getLogger(getClass());

  /**
   * 测试日志输出
   */

   @Test
   void logTest() {
     logger.trace("trace 级别日志");
     logger.debug("debug 级别日志");
     logger.info("info 级别日志");
     logger.warn("warn 级别日志");
     logger.error("error 级别日志");
   }
}
```

Spring Boot 日志默认级别为 info，日志输出内容默认包含以下元素

- 时间日期
- 日志级别
- 进行 ID
- 分隔符 ---
- 线程名，方括号括起来
- Logger 名称
- 日志内容

## 修改默认日志配置

在 application.properties 中，修改 Spring Boot 日志的默认配置

```yml
# 日记级别
logging.level.net.biancheng.www=trace
# 使用相对路径的方式设置日志输出的位置（项目根目录/my-log/mylog/spring.log）
logging.file.path=my-log/mylog

# 控制台日志输出格式
logging.pattern.console=%d{yyyy-MM-dd hh:mm:ss} [%thread] %-5level %logger{50} - %msg%n

# 日志文件输出格式
logging.pattern.file=%d{yyyy-MM-dd} === [%thread] === %-5level === %logger{50} === - %msg%n
```
