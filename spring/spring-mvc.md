# Spring MVC

MVC 思想将一个应用分成 3 个基本部分，即 Model(模型)、View(视图)、Controller(控制器)，让这 3 个部分以低耦合
进行协同工作，从而提高应用的可扩展性以及可维护性

Spring MVC 是一款优秀的基于 MVC 思想的应用框架，它是 Spring 提供的一个实现了 Web MVC 设计模式的轻量级 Web 框架

MVC 是 Model、View 和 Controller 的缩写，分别代表 Web 应用程序中的 3 种职责

- 模型，用于存储数据以及处理用户请求的业务逻辑

- 视图，向控制器提交数据，显示模型中的数据

- 控制器，根据视图提出的请求判断将请求和数据交给哪个模型处理，将处理后的有关结果交给哪个视图更新显示

### 基于 Servlet 的 MVC 模式

- 模型，一个或多个 JavaBean 对象，用于存储数据(实体模型，由 JavaBean 类创建)和处理业务逻辑(业务模型，由一般的 Java 类创建)

- 视图，一个或多个 JSP 页面，向控制器提交数据和为模型提供数据显示，JSP 页面主要使用 HTML 标记和 JavaBean 标记显示数据

- 控制器，一个或多个 Servlet 对象，根据视图提交的请求进行控制，即将请求转发给处理业务逻辑的 JavaBean，并将处理结果存放到
  实体模型 JavaBeab 中，输出给视图显示

**流程图**

```

         请求                        请求
      ----------->             ------------->   Servlet 控制器
                                                 |        (入)
                    HTML/JSP                     |        ｜
浏览器                视图                                  |
                                               存储数据  处理数据
                                                 |
                                                 |         |
          响应                    显示数据        (入)       |
      <-----------            <-------------    JavaBean 模型     <-------->  数据库
```

Spring MVC 框架是高度可配置的，包含多种视图技术。Spring MVC 框架并不关心使用的视图技术

Spring MVC 框架主要由 DispatcherServlet、处理器映射、控制器、视图解析器、视图组成

Spring MVC 所有的请求都经过 DispatcherServlet 来统一分发，在 DispatcherServlet 将请求分发给
Controller 之前需要借助 Spring MVC 提供的 HandlerMapping 定位到具体的 Controller

Controller 接口将处理用户请求，一旦 Controller 处理完用户请求，将返回 ModelAndView 对象给 DispatcherServlet
前端控制器，ModelAndView 中包含了模型和视图

从宏观角度看，DispatcherServlet 是整个 Web 应用的控制器，从微观考虑，Controller 是单个 Http 请求处理过程中的控制器

在使用 Spring MVC 进行 Web 应用开发时 Controller 是 Web 应用的核心，Controller 实现类包含了对用户请求的处理逻辑，
是用户请求和业务逻辑之间的桥梁，是 Spring MVC 框架的核心部分，负责具体的业务逻辑处理

## 基于注解的控制器

基于注解的控制器具有以下两个优点

- 基于注解的控制器可以编写处理多个处理方法，进而可以处理多个请求，这就允许将相关的操作编写在同一个控制器类中，从而减少控制器
  类的数量，方便以后维护

- 基于注解的控制器不需要在配置文件中部署映射，仅需要使用`RequestMapping`注释类型注解一个方法进行请求处理

Spring MVC 中最重要的两个注解类型是 Controller 和 RequestMapping

### RequestMapping 注解类型

在基于注解的控制器类中可以为每个请求编写对应的处理方法。如何将请求与处理方法一一对应呢？

**方法级别注解**

```java
package controller;


@Controller
public class IndexController {

  @RequestMapping(value = "/index/login")
  publice String login() {
    // login 代表逻辑视图名称，需要根据Spring MVC 配置
    return "login";
  }

  @RequestMapping(value = "/index/register")
  public String register() {
    return "register";
  }
}
```

注解的 value 属性将请求 URI 映射到方法，value 属性是 RequestMapping 注解的默认属性，如果只有一个`value`属性，
则可以省略该属性

```
http://localhost:8080/ch10/index/login
```

**类级别注解**

```java
package controller;


@Controller
@RequestMapping("/index")
public class IndexController {
  @RequestMapping("/login")
  public Sting login() {
    return "login";
  }

  @RequestMapping("/register")
  public String register() {
    return "register";
  }
}
```

在类级别注解的情况下，控制器类中的所有方法都将映射为类级别的请求。为了方便维护程序，建议开发者采用类
级别的注解，将相关处理放在同一个控制器类中。例如，对商品的增、删、改、查处理方法都放在`GoodsOperate`控制类中

## 编写请求处理方法

在控制类中每个请求处理方法可以有多个不同类型的参数，以及一个多种类型的返回结果

如果需要在请求处理方法中使用 Servlet API 类型，可以将这些类型作为请求处理方法的参数类型。除了 Servlet API
参数类型以外，还有输入输出流、表单实体类、注解类型、与 Spring 框架相关的类型等

### 请求处理方法常见的返回类型

最常见的返回类型就是代表逻辑视图名称的 String 类型，还有`ModelAndView`、Model、View 以及任意的 Java 类型

## Controller 接收请求参数的常见方式

Controller 接收请求参数的方式有很多种，有的适合 get 请求方式，有的适合 post 请求方式，有的两者都适合

- 通过实体 Bean 接收请求参数

- 通过处理方法的形参接收请求参数

- 通过 HttpServletRequest 接收请求参数

- 通过`@PathVariable`接收 URL 中的请求参数

### 通过@RequestParam 接收请求参数

通过@RequestParam 接收请求参数适用于 get 和 post 提交请求方式。通过@RequestParam 接收请求参数
与`通过处理方法的形参接收请求参数`的区别，当请求参数与接收参数名不一致时，@RequstParam 接收请求参数会报 404 错误

### 通过@ModelAttribute 接收请求参数

@ModelAttribute 注解放在处理方法的形参上时，用于将多个请求参数封装到一个实体对象，从而简化数据绑定流程，
而且自动暴露为模型数据

`将多个请求参数封装为一个实体对象`，并不能暴露为模型数据

## 重定向与转发

重定向是将用户从当前处理请求定向到另一个视图或处理请求，以前的请求中存的信息全部失效，并进入一个新的 request 作用域。

转发是将用户对当前处理的请求转发给另一个视图或处理请求，以前的 request 中存放的信息不会失效

转发是服务器行为，重定向是客户端行为

**转发过程**

客户端浏览器发送 http 请求，web 服务器接收此请求，调用内部的一个方法在容器内部完成请求处理和转发动作，将目标资源发送
给客户。在这里转发的路径必须是同一个 Web 容器下的 URL，其不能转向到其他的 Web 路径上，中间传递的是自己的容器内的 request。
在客户浏览器的地址栏中显示的仍然是其第一次访问的路径，也就是说客户是感觉不到服务器做了转发的。转发行为浏览器只做了一次访问请求

**重定向过程**

客户浏览器发送 http 请求，Web 服务器接收后发送 302 状态码响应及对应新的 location 给客户浏览器，客户浏览器发现是 302 响应，
则自动再发送一个新的 http 请求，请求 URL 是新的 location 地址，在这里 location 可以重定向到任意 URL，既然是浏览器重新
发出了请求，那么就没有什么 request 传递的概念了。
在客户浏览器的地址栏中显示了其重定向的路径，客户可以观察到地址的变化。重定向行为浏览器做了至少两次访问请求

### @ModelAttribute 注解

被@ModelAttribute 注解的方法将在每次调用该控制器类的请求处理方法前被调用。这种特性可以用来控制登录权限，
当然控制登录权限的方法有很多，例如拦截器、过滤器等
