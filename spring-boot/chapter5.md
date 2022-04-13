# 多环境配置

在实际的项目开发中，一个项目通常会存在多个环境，例如，开发环境、测试和生产环境等。不同环境的配置
也不尽相同，例如开发环境使用的是开发数据库，生产环境使用的是线上的正式数据库

Profile 为不同环境下使用不同的配置提供了支持，我们可以通过激活、指定参数等方式快速切换环境

## 多 Profile 文件方式

Spring Boot 的配置文件共有两种形式，`.properties`文件和`.yml`文件，都可以通过文件名的命名形式区分出不同的
环境的配置，文件命名格式为

```
application-{profile}.properties/yml
```

其中，{profile}一般为各个环境的名称或简称，例如 dev、test、prod 等等

## properties 配置

在`src/main/resources`下添加 4 个配置文件

- application.properties 主配置文件

- application-dev.properties 开发环境配置文件

- application-test.properties 测试环境配置文件

- application-prod.properties 生产环境配置文件

在 application.properties 文件中，指定默认服务端口号，并配置激活生产环境的 profile

```
# 默认端口号
server.port=8080

# 激活指定环境
spring.profiles.active=prod
```

## 命令行激活 Profile

我们将 Spring Boot 项目打包成 JAR 文件，并通过命令行运行，配置命令行参数

- 执行 mvn 命令将项目打包

```shall
mvn clean package
```

打开命令行窗口，跳转到 JAR 文件所在目录，执行以下命令

```
java -jar helloworld-0.0.1-SNAPSHOT.jar --spring-profiles.active=dev
```
