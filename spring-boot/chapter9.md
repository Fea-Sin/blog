# spring-boot-starter-web

Spring MVC 是 Spring 提供的一个基于 MVC 设计模式的轻量级 Web 开发框架，其本身就是 Spring 框架的
一部分，可以与 Spring 无缝集成，性能方面具有先天的优越性，是当今业界主流的 Web 开发框架之一

Spring Boot 是在 Spring 的基础上创建的一款开源框架，它提供了 spring-boot-starter-web 来为 Web 开发予以支持。
spring-boot-starter-web 为我们提供了嵌入的 Servlet 容器以及 SpringMVC 的依赖，并为 Spring MVC 提供了大量自动配置，
可以适用于大多数 Web 开发场景

- 引入了 ContentNegotiatingViewResolver 和 BeanNameViewResolver（视图解析器）

- 对包括 WebJars 在内的静态资源支持

- 自动注册 Converter、GenericConverter 和 Formatter(转换器和格式化器)

- 对 HttpMessageConverters 的支持，Spring MVC 中用于转换 HTTP 请求和响应的消息转换器

- 自动注册 MessageCodesResolver，用于定义错误代码生成规则

- 支持对静态首页 index.html 的访问

- 自动使用 ConfigurableWebBindingInitializer

只要我们在 Spring Boot 项目中的 pom.xml 中引入 spring-boot-starter-web，即使不进行任何配置，也可以直接使用
Spring MVC 进行 Web 开发

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

在 demo 包下创建一个名为 HelloController

```java
package net.biancheng.www.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@controller
public class HelloController {
  @ResponseBody
  @RequestMapping("/hello")
  public String hello() {
    return "www.biancheng.net";
  }
}
```

> spring-boot-starter-web 默认引入了核心启动器 spring-boot-starter

# Spring Boot 静态资源映射

在 Web 应用中会涉及到大量的静态资源，例如 JS、CSS、HTML 等，Spring MVC 导入静态资源文件时，需要配置静态
资源的映射，SpringBoot 已经默认完成了这一工作

Spring Boot 默认为我们提供了 3 中静态资源映射规则

- WebJars 映射
- 默认资源映射
- 静态首页映射

## WebJars 映射

Web 应用中通常会使用大量的 JS 和 CSS，例如 jQuery、Bootstrap 等等，通常我们会将这些 Web 前端资源拷贝到
Java Web 项目的 webapp 相应目录下进行管理，但是 Spring Boot 项目是以 JAR 包的形式进行部署的，不存在 webapp
目录

WebJars 可以完美的解决上面的问题，可以以 Jar 形式为 Web 项目提供资源文件

WebJars 可以将 Web 前端资源等打包成一个个 Jar 包，然后将这些 Jar 包部署到 Maven 中央仓库进行统一管理，当
Spring Boot 项目中需要引入 Web 前端资源时，将其导入到项目中即可

所有通过 WebJars 引入的前端资源都放在当前类路径(classpath)下的 `/META-INF/resources/webjars/`目录中

例如在 pom.xml 中添加 jQuery 依赖，将 jQuery 引入到项目中

## 默认静态资源映射

Spring Boot 会从以下路径中查找资源文件，优先级依次降低

1 classpath:/META-INF/resources/

2 classpath:/resources/

3 classpath:/static/

4 classpath:/public/

这些路径又被称为静态资源文件夹

> 访问静态首页或欢迎页面时，其查找顺序也遵循默认静态资源的查找顺序，即先查找优先级高级的目录，再查找优先级低的目录
> 直到找到 index.html 为止

# Spring Boot 整合 Thymeleaf

Spring Boot 推荐使用 Thmeleaf 作为其模版引擎，Spring Boot 为 Thymeleaf 提供了一系列默认配置，项目中一旦
导入了 Thymeleaf 的依赖，相应的自动配追就会自动生效

Spring Boot 整合 Thymeleaf 模版引擎，需要以下步骤

- 引入 Starter 依赖

- 创建模版依赖，并放在指定目录下

## 引入依赖

Spring Boot 整合 Thymeleaf 的第一步，在项目的 pom.xml 中添加 Thymeleaf 的 starter 依赖

```xml
<!-- Thymeleaf 启动器 -->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

## 模版文件

Thymeleaf 模版的默认位置在`resources/templates`目录下，默认的后缀是 html，即只要将 HTML 页面放在
`classpath:/templates`下，Thymeleaf 就能自动进行渲染

创建一个 hello.html 的页面，比将该页面放在项目类路径 resources 下的 template 目录中

```html
<!DOCTYPE html>
<!-- 导入 thymeleaf的名称空间 -->
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
    <title>Hello</title>
  </head>
  <body>
    <h1 th:text="'欢迎来到' + ${name}"></h1>
  </body>
</html>
```

新建一个控制类 HelloController，比通过参数 map 传递数据到前台页面中

```java
package net.biancheng.www.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.Map;

@Controller
public class HelloController {
  @RequestMapping("/hello")
  public String hello(Map<String, Object> map) {
    // 通过map向前台页面传递数据

    map.put("name", "编程帮（www.biancheng.net）")
    return "hello";
  }
}
```
