# Spring Boot 注册 Web 原生组件

Spring Boot 默认以 Jar 包方式部署的，默认没有 web.xml，因此无法像以前一样通过 web.xml 配置来使用
Servlet、Filter、Listener，Spring Boot 提供了 2 种方式来注册这些 Web 原生组件

- 通过组件扫描注册

- 使用 RegistrationBean 注册

@WebServlet 用于声明一个 Servlet

@WebFilter 用于声明一个 Filter

@WebListener 用于声明一个 Listener

这些注解可直接标注在对应的组件上，它们与在 web.xml 中配置意义相同

想要在 Spring Boot 中注册这些原生 Web 组件，可以使用@ServletComponentScan 注解实现，该注解可以扫描
标记@WebServlet、@WebFilter、@WebListener 三个注解的组件类，并将它们注册到容器中

> @ServletComponentScan 注解只能标记在启动类或配置类上

## 使用@WebServlet 注解声明一个自定义的 Servlet

```java
package net.biancheng.www.servlet;

import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.io.IOException;
import javax.io.PrintWriter;

// 使用@WebServlet 注解声明一个Servlet
@WebServlet(name = "myServlet", urlPatterns = "/myServlet")
public class MyServlet extends HttpServlet {
  @Override
  protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    doPost(req, resp);
  }

  @Override
  protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    resp.setContentType("text/html;charset=UTF-8");
    PrintWriter writer = resp.getWriter();
    writer.write("Spring Boot Servlet");
    writer.close();
  }
}
```

在启动类上使用@ServletComponentScan 注解

```java
package net.biancheng.www;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.ServletComponentScan;

@ServletComponentScan
@SpringBootApplication
public class SpringBootServletApplication {

   public static void main(String[] args) {
     SpringApplication.run(SpringBootServletApplication.class, args)
   }
}
```

## 使用 RegistrationBean 注册

我们还可以在配置类中使用 RegistrationBean 类注册原生 Web 组件，RegistrationBean 是个抽象类，负责将
组件注册到 Servlet 容器中，Spring 提供了三个它的实现类

ServletRegistrationBean Servlet 的注册类
FilterRegistrationBean Filter 的注册类
ServletListenerRegistrationBean Listener 的注册类
