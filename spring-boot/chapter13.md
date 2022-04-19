# Spring Boot 异常处理

在日常 Web 开发中，经常会遇到大大小小的异常，此时往往需要一个统一的异常处理机制，来保证客户端接收较为友好的提示，
Spring Boot 同样提供了一套默认的异常处理机制

一旦程序出现异常，Spring Boot 会自动识别客户端类型(浏览器客户端或机器客户端)，并根据客户端的不同，以不同的形式
展示异常信息

对于浏览器而言，Spring Boot 会响应一个`WhiteLabel Error Page`

对于机器客户端而言，Spring Boot 将生成 JSON 响应，来展示异常信息

```json
{
  "timestamp": "2021-07-12T07:05:29.885+00:00",
  "status": 404,
  "error": "Not Found",
  "message": "No message available",
  "path": "/main.html"
}
```

## Spring Boot 异常处理自动配置原理

Spring Boot 通过配置类 ErrorMvcAutoConfiguration 对异常处理提供了自动配置，该配置类向容器中注入
了以下 4 个组件

- ErrorPageCustomizer: 该组件会在系统发生异常后，默认将请求转发到`/error`上

- BasiErrorController: 处理默认的`/error`请求

- DefaultErrorViewResolver: 默认的错误视图解析器，将异常信息解析到相应的错误视图上

- DefaultErrorAttributes: 用于页面上共享异常信息

## ErrorPageCustomizer

ErrorPageCustomizer 通过 registerErrorPages()方法来注册错误页面的响应规则，当系统中发生异常后，
ErrorPageCustomizer 组件自动生效，并将请求转发到`/error`上，交给`BasicErrorController`进行处理

## BasicErrorController

Spring Boot 通过 BasicErrorController 进行统一的错误处理，Spring Boot 会自动识别发出请求的客户端类型，
根据客户端类型，将请求分别交给`errorHtml()`或`error()`方法进行处理

errorHtml(HttpServletRequest request, HttpServletResponse response)， ModelView，浏览器客户端，
text/html 错误页面

error(HttpServletRequest request)，ResponseEntity<Map<String, Object>>，机器客户端，例如安卓、IOS、Postman 等等，JSON

## DefaultErrorViewResolver

ErrorMvcAutoConfiguration 还向容器中注入了一个默认的错误视图解析器组件 DefaultErrorViewResolver

DefaultErrorViewResolver 解析异常信息的步骤

1 根据错误状态码(404, 500, 400 等)，生成一个错误视图`error/status`，例如 error/404, error/500, error/400

2 尝试使用模版引擎解析`error/status`视图，即尝试从 classpath 类路径下的 templates 目录下，查找 error/status.html

3 若模版引擎能够解析到 error/status 视图，则将视图和数据封装成 ModelAndView 返回并结束整个解析流程，否则跳转到 4 步

4 依次从各个静态资源文件中查找 error/status.html，若在静态文件中找到，则返回并结束整个解析流程，否则跳转到第 5 步

5 将错误状态码转换为`4xx`或`5xx`，重复前 4 个步骤，若解析成功则返回并结束，而否跳转到第 6 步

6 处理默认的`/error`请求，使用 Spring Boot 默认错误页面

## DefaultErrorAttributes

DefaultErrorAttributes 可以从请求中获取异常或错误信息，并将其封装为一个 Map 对象，以 JSON 数据形式返回

# Spring Boot 全局异常处理

Spring Boot 已经提供了一套默认的异常处理机制，但是默认处理机制并不一定适用我们的实际业务场景，我们通常
会根据自身需要对 Spring Boot 全局异常进行统一定制，例如错误页面，定制错误数据等

## 定制错误页面

通过以下 3 种方式定制 Spring Boot 错误页面

自定义 error.html

自定义动态错误页面

自定义静态错误页面

在模版引擎文件夹`classpath:/resources/templates`下，创建一个 error.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8" />
    <title>自定义 error.html</title>
  </head>
  <body>
    <h1>自定义 error.html</h1>
    <p>
      status：
      <span th:text="${status}"></span>
    </p>
    <p>
      error：
      <span th:text="${error}"></span>
    </p>
    <p>
      timestamp：
      <span th:text="${timestamp}"></span>
    </p>
    <p>
      message：
      <span th:text="${message}"></span>
    </p>
    <p>
      path：
      <span th:text="${path}"></span>
    </p>
  </body>
</html>
```

## 精确匹配错误页面

我们可以根据错误状态码的不同，分别创建不同的错误页面，例如 404.html、500.html、400.html

## 定制错误数据

Spring Boot 提供了一套默认的异常处理机制，其主要流程如下

发生异常时，将请求转发到`/error`，交由 BasicErrorController 进行处理，BasicErrorController
根据客户端的不同，自动适配返回的响应形式，BasicErrorController 处理异常时，会调用 DefaultErrorAttributes
的 getErrorAttributes()方法获取错误数据

我们可以定制 Spring Boot 的错误数据，通过继承 DefaultErrorAttributes 来定义一个错误属性工具，
并在原来的基础上添加自定义的错误数据

## 自定义异常处理

被@ControllerAdvice 注解的类可以用来实现全局异常处理，这是 Spring MVC 中提供的功能

自定义异常，在 exception 包内，创建一个名为 UserNotExitException 的异常类

```java
package net.biancheng.www.exception;

/**
 * 定义异常
 */

public class UserNotExistException extends RuntimeException {
  public UserNotExistException() {
    super("用户不存在！")
  }
}
```

在 IndexController 添加如下方法，触发 UserNotExistException 异常

```java
@Controller
public class IndexController {
  ...

  @GetMapping(value = {"/testException"})
  public String testException(String user) {
    if ("user".equals(user)) {
      throw new UserNotExistException();
    }
    // 跳转到登录页 login.html
    return "login";
  }
}
```

在 controller 包中创建一个名为 MyExceptionHandler 异常处理类

```java
package net.biancheng.www.controller;

import net.biancheng.www.exception.UserNotExistException;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

import javax.servlet.http.HttpServletRequest;
import java.util.HashMap;
import java.util.Map;

@ControllerAdvice
public class MyExceptionHandler {
  @ExceptionHandler(UserNotExistException.class)
  public String handleException(Exception e, HttpServletRequest request) {
    Map<String, Object> map = new HashMap<>();

    request.setAttribute("javax.servlet.error.status", 500);
    map.put("code", "user.notexist");
    map.put("message", e.getMessage());

    // 将自定义错误数据传入 request 域中
    request.setAttribute("ext", map);
    return "forward:/error";
  }
}
```

自定义错误属性处理工具

在 component 包中，创建一个错误处理工具 MyErrorAttributes 继承 DefaultErrorAttributes

```java
package net.biancheng.www.component;

import org.springframework.boot.web.error.ErrorAttributeOptions;
import org.springframework.boot.web.servlet.error.DefaultErrorAttributes;
import org.springframework.stereotype.Component;
import org.springframework.web.context.request.WebRequest;

import java.util.Map;

@Component
public class MyErrorAttributes extends DefaultErrorAttributes {
  @Override
  public Map<String, Object> getErrorAttributes(WebRequest webRequest, ErrorAttributeOptions options) {
    Map<String, Object> errorAttributes = super.getErrorAttributes(webRequest, options);

    errorAttributes.put("company", "www.biancheng.net");

    Map ext = (Map) webRequest.getAttribute("ext", 0);
    errorAttributes.put("ext", ext);

    return errorAttributes;
  }
}
```
