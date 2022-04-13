# 拦截器

在开发一个网站时可能有这样的需求，某些页面只希望几个特定的用户浏览，对于这样的访问权限控制，拦截器可以
实现上述需求

## Spring 拦截器

在 Spring MVC 框架中定义一个拦截器需要对拦截器进行定义和配置，定义一个拦截器可以通过两种方式，一种是
通过实现`HandlerInterceptor`接口或继承`HandlerInterceptor`接口的实现类来定义

另一种是通过实现`WebRequestInterceptor`接口或继承`WebRequestInterceptor`接口的实现类来定义

`HandlerInterceptor`接口的 3 个方法

- `preHandle`方法，该方法在控制器的处理请求方法前执行，其返回值表示是否中断后续操作，返回 true 表示继续
  向下执行，返回 false 表示中断后续操作

- `postHandle`方法，该方法在控制器的处理请求方法调用之后、解析视图之前执行，可以通过此方法对请求域中的模型
  和视图进一步的修改

- `afterCompletion`方法，该方法在视图渲染结束后执行，可以通过此方法实现一些资源清理、记录日志信息等工作

## 数据验证

数据验证分为客户端验证和服务器端验证，客户端验证主要是过滤正常用户的误操作，通过 JavaScript 代码完成。服务端验证
是整个应用阻止非法数据的最后防线，通过在应用中编程实现

## 国际化

国际化是商业软件系统的一个基本要求，因为当今的软件系统要面对全球的浏览者，国际化的目的是根据用户的语言环境的不同向
用户输出与之相应的页面

### Java 国家化的思想

Java 国际化的思想是将程序中的信息放在资源文件中，程序根据支持的国家及语言环境读取相应的资源文件。资源文件是`key-value`
对，每个资源文件中的 key 是不变的，但 value 随不同国家/语言变化

Spring MVC 的国际化是建立在 Java 国际化的基础之上的，Sping MVC 框架的底层国际化与 Java 国际化是一致的，作为一个
良好的 MVC 框架，Spring MVC 将 Java 国际化的功能进行了封装和简化，开发者使用起来会更加简单、快捷

## 统一异常处理

通过本章学习，读者能够掌握 Spring MVC 框架统一异常处理的使用方法

- 简单异常处理 SimpleMappingExceptionResolver

- 实现 HandlerExceptionResolver 接口自定义异常

- 使用@ExceptionHandler 注解实现异常处理

在 Spring MVC 应用的开发中，不管是对底层数据库操作，还是业务层或控制层操作，都会不可避免地遇到
各种可预知的、不可预知的异常需要处理。如果每个过程都单独处理异常，那么系统的代码耦合度高，工作量大
且不好统一，以后维护的工作量也很大

如果能将所有类型的异常处理从各层中解耦出来，这样即保证了相关过程的功能单一，又实现了异常信息的统一
处理和维护。幸运的是，Spring MVC 框架支持这样的实现

### HandlerExceptionResolver 接口

`org.springframework.web.servlet.HandlerExceptionResolver`接口用于解析请求处理过程中所产生
的异常。开发者可以开发该接口的实现类进行 Spring MVC 应用的异常统一处理

### ExceptionHandler 注解

创建 BaseController 类，并在该类中使用`@ExceptionHandler`注解声明异常处理方法。
将所有需要异常处理的 Controller 都继承 BaseController 类

```java
public abstract class BaseController {
  // 基于@ExceptionHandler异常处理
  @ExceptionHandler
  public String exception(HttpServletRequest request, Exception ex) {
    request.setAttribute("ex", ex)
    // 根据不同错误转向不同处理
    if (ex instanceof SQLException) {}
    else if (ex instanceof MyException) {}
    else {
      return "error";
    }
  }
}
```

```java
@Controller
public class TestExceptionController extends BaseController {
  // ...
}
```

使用`@ExceptionHandler`注解实现异常处理具有集成简单、可扩展性好(只需将要异常处理的 Controller 继承于 BaseController 即可)
不需要附加 Spring 配置等优点，但该方法对已有的代码存在入侵性，需要修改已有的代码，使相关类继承于 BaseController
