# 类型转换和格式化

在 Spring MVC 框架中需要收集用户请求参数，并将请求参数传递给应用的控制器组件。此时存在一个问题，
即所有的请求参数只能是字符串数据类型，但 Java 是强类型语言，所以 Spring MVC 框架必须将这些字符串
请求参数转换成相应的数据类型

Spring MVC 框架不仅提供了强大的类型转换和格式化机制，而且开发者还可以方便地开发出自己的类型转换器和格式化
转换器，完成字符串和各种数据类型之间的转换

类型转换是在视图和控制器相互传递数据时发生的。Spring MVC 框架对于基本类型(int, long, float, double, boolean, char)等
已经做好了基本类型转换

> 在使用内置类型转换器时，请求参数输入值与接收参数类型要兼容，否则会报 400 错误

## 自定义类型转换器

当输入`apple,10.58,200`时希望程序自动创建一个 new Goods，并将`apple`值自动赋值给 goodname 属性，将`10.58`值自动赋值给
goodprice 属性，将`200`值自动赋值给 goodnumber 属性

如果想实现上述应用，需要做以下 5 件事

- 创建实体类

- 创建控制器类

- 创建自定义类型转换器类

- 注册类型转换器

- 创建相关视图

## Formatter

Spring MVC 框架的 Formatter<T> 与 Converter<S,T>一样，也是一个可以将一种数据类型转换为另一种数据
类型的接口。不同的是 Formatter<T>的源数据类型必须是 String 类型，而 Converter<S,T>的源数据类型是任意数据类型

## 数据绑定

数据绑定是将用户参数输入值绑定到领域模型的一种特性，在 Spring MVC 的 Controller 和 View 参数数据传递中所有的
HTTP 请求参数的类型均为字符串，如果模型需要绑定为 double 和 int，则需要手动进行类型转换，而有了数据绑定后就不再需要
手动将 HTTP 请求中的 String 类型转换为模型需要的类型

## JSON 概述

JSON(JavaScript Object Notation)JS 对象标记，是一种轻量级的数据交换格式，与 XML 一样，JSON 也是基于纯文本的数据格式。
它有对象结构和数组结构两种数据结构

**对象结构**

对象结构以`{`开始以`}`结束，中间由 0 个或多个以`,`分隔的`key/value`对构成

**数组结构**

数组结构以`[`开始以`]`结束，中间由 0 个或多个以`,`分隔的值的列表组成

对象和数组数据结构也可以分别组成更复杂的数据结构

## JSON 数据转换

为了实现浏览器与控制器类之间的 JSON 数据交互，Spring MVC 提供了默认的处理 JSON 格式请求响应

在使用注解开发时需要用到两个重要的 JSON 格式转换注解，分别是`@RequestBody`和`@ResponseBody`

- `@RequestBody` 用于将请求体中的数据绑定到方法的形参中，该注解应用在方法的形参上

- `@ResponseBody` 用于直接返回 return 对象，该注解应用在方法上

```java
package controller;

@Controller
public class TestController {
  // 接收页面请求的JSON数据，并返回JSON格式的结果
  @RequestMapping("/testJson")
  @ResponseBody
  public Person testJson(@RequestBody Person user) {
    // 打印接收的JSON数据
    System.out.println("pname=" + user.getPname() + ", password=" + user.getPassword() + ", page=" + user.getPage())

    return user;
  }
}
```
