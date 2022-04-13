# Spring Boot 定制 Spring MVC

Spring Boot 抛弃了传统的 xml 配置文件，通过配置类，标注@Configuration 的类相当于一个 xml 配置文件，以
JavaBean 形式进行相关配置

Spring Boot 对 Spring MVC 的自动配置可以满足我们的大部分需求，但是我们也可以通过自定义配置类(标注@Configuration 的类)
并实现 WebMvcConfigurer 接口来定制 Spring MVC 配置，例如拦截器、格式化程序、视图控制器等等

WebMvcConfigurer 是一个基于 Java8 的接口，该接口定义了许多与 Spring MVC 相关的方法，其中大部分方法都是 default 类型的，
且都是空实现，因此我们只需要定义一个配置类实现 WebMvcConfigurer 接口，并重写相应的方法便可以定制 Spring MVC 的配置

在 Spring Boot 项目中，我们可以通过以下 2 种形式定制 Spring MVC

- 扩展 Spring MVC

- 全面接管 Spring MVC

## 扩展 Spring MVC

如果 Spring Boot 对 Spring MVC 的自动配置不能满足我们的需要，我们可以通过自定义一个 WebMvcConfigurer 实现
WebMvcConfigurer 接口的配置类（标注@Configuration）来扩展 Spring MVC，这样不但能够保留 Spring Boot 对
Spring MVC 的自动配置，享受 Spring Boot 自动配置带来的便利，还能额外自定义 Spring MVC 配置

创建一个名为 MyMvcConfig 的配置类并实现 WebMvcConfigurer 接口，重写 addViewControllers()方法

```java

package net.biancheng.www.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.*;

// 实现WebMvcConfigurer 接口可以来扩展SpringMVC 的功能

@Configuration
public class MyMvcConfig implements WebMvcConfigurer {

  @Override
  public void addViewControllers(ViewControllerRegistry registry) {
    // 当访问 '/' 或 '/index.html'时，都直接跳转到登录页面

    registry.addViewController("/").setViewName("login");
    registry.addViewController("/index.html").setViewName("login");
  }
}
```

创建一个名为 IndexController 的 Controller

```java
package net.biancheng.www.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestMapping;


@Controller
public class IndexController {
  /**
   * 跳转到登录页面
   *
   */

  @GetMapping(value = {"/login"})
  public String loginPage() {
    // 跳转到登录页面 login.html
    return "login";
  }
}
```

## 全面接管 Spring MVC

在一些特殊情况下，我们可能需要抛弃 Spring Boot 对 Spring MVC 的全部自动配置，完全接管 Spring MVC，此时我们
可以自定义一个 WebMvcConfigurer 类型（实现 WebMvcConfigurer 接口）的配置类，并在该类上标注@EnableWebMvc 注解，
来实现完全接管 Spring MVC

完全接管 Spring MVC 后，Spring Boot 对 Spring MVC 的自动配置将全部失效

Spring Boot 能够访问位于静态资源文件夹中的静态文件，这是在 Spring Boot 对 Spring MVC 的默认自动配置中定义的，当我们
全面接管 Spring MVC 后，Spring Boot 对 Spring MVC 的默认配置都失效，此时再访问静态资源文件中的资源就会报 404 错误
