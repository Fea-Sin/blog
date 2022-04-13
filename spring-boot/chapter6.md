# Spring Boot 默认配置文件

通常情况下，Spring Boot 在启动时将 resources 目录下的 application.properties 或 application.yml
作为其默认配置文件，我们可以在该配置文件中对项目进行配置

Spring Boot 项目中可以存在多个 application.properties 或 application.yml，Spring Boot 启动时会扫描
5 个位置，并将它们作为 Spring Boot 的默认配置文件

```
1 file:./config/

2 file:./config/*/

3 file:./

4 classpath:/config/

5 classpath:/
```

> file 指当前项目根目录，classpath 指当前项目的类路径即 resources 目录

以上所有位置的配置文件都会被加载，且它们的优先级以次降低，序号越小优先级越高。其次，位于相同位置的 application.properties
的优先级高于 application.yml

所有位置的文件都会被加载，高优先级配置会覆盖低优先级配置，形成互补配置

根据 Spring Boot 默认配置文件优先级进行分析，根目录下`/config`目录下的配置文件优先级最高

# Spring Boot 外部配置文件

除了默认配置文件，Spring Boot 还可以加载一些位于项目外部的配置文件。我们可以通过指定外部配置文件的路径

- spring.config.location

- spring.config.additional-location

## spring.config.location

我们先将 Spring Boot 项目打包成 JAR 文件，然后在命令行启动命令中，使用命令行参数 `--spring.config.location`，
指定外部配置文件的路径

```
java -jar --spring.config.location={外部配置文件全路径}
```

例如

```
java -jar springbootdemo-0.0.1-SANPSHOT.jar --spring.config.location=D:\myConfig\my-application.yml
```

需要注意的是，使用该参数指定配置文件后，会使项目默认配置文件失效

## spring.config.additional-location

`--spring.config.addtional-location`不会使项目默认的配置失效，使用该命令行参数添加的外部配置
文件会与项目默认的配置文件共同生效，形成互补配置，且其优先级是最高的

注意，Maven 对项目进行打包时，位于项目目录下的配置文件是无法被打包进项目的 JAR 包的，因此位于根目录下的默认
配置文件无法在 JAR 中生效，即该项目将只加载指定的外部配置文件和项目类路径(classpath)下的默认配置文件

它们的优先级顺序为，由高到低

1 spring.config.addtional-location 指定的外部配置文件

2 classpath:/config/application.yml

3 classpath:/application.yml

# Spring Boot 配置加载顺序

Spring Boot 不仅可以通过配置文件进行配置，还可以通过环境变量、命令行参数等多种形式进行配置。这些配置都可以让
开发人员在不修改代码的前提下，直接将一套 Spring Boot 应用程序在不同的环境中运行

Spring Boot 配置形式及加载顺序，优先级由高到低

1 命令行参数

2 来自 java:comp/env 的 JNDI 属性

3 Java 系统属性(System.getProperties())

4 操作系统环境变量

5 RandomValuePropertySource 配置的 `random.*`属性值

6 配置文件(YAML 文件、Properties 文件)

7 @Configuration 注解类上的@PropertySource 指定的配置文件

8 通过 SpringApplication.setDefaultProperties 指定的默认属性

以上所有形式的配置都会被加载，当存在相同配置内容时，高优先级的配置会覆盖低优先级的配置，高优先级和低优先级的配置
内容取并集，共同生效，形成互补配置

## 命令行参数

Spring Boot 中的所有配置，都可以通过命令行参数进行指定

```
java -jar {Jar文件名} --{参数1}={参数值1} --{参数2}={参数值2}
```

## 配置文件

创建一个 mybootdemo 的 Spring Boot 项目，并在`src/main/resources` 下创建以下 4 个配置文件

application.yml 默认配置

application-dev.yml 开发环境

application-test.yml 测试环境

application-prod.yml 生产环境
