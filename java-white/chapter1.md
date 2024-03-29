# Java 程序

在 Java 中，程序都是以类的方式组织的，Java 源文件都保存为`.java`文件中，每个可运行的
程序都是一个类文件，或者称之为字节码文件，保存为`.class`文件

在 Java 中一个源文件中可以定义多个类，但需要注意如下问题

- 只能有一个类可以声明为公有(public)的

- 文件命名必须与公有类名完全一致，包括字母大小写

- public static void main(String[] args) 只能定义在公有类中

作为一个 Java 应用程序，类中必须包含静态 main 方法，程序执行是从 main 方法开始的

```java
public static void main(String[] args)
```

## 标识符

标识符就是变量、常量、方法、枚举、类、接口等由程序员指定的名字。构成标识符的字母均有一定的规范，Java 语言中标识符的命名
规则如下

- 区分大小写，Myname 与 myname 是两个不同的标识符

- 首字符，可以是下划线(`_`)、美元符、字母，但不能是数字

- 除了首字符外其他字符，可以是下划线(`_`)、美元符、字母、数字

- 关键字不能作为标识符

> Java 语言中字母采用的是双字节 Unicode 编码，Unicode 叫作统一编码制，它包含了亚洲文字编码，如中文、日文、韩文等
>
> Unicode 是国际组织制定的可以容纳世界上所有文字和符号的字符编码方案

## 关键字

关键字由语言本身定义好的，不能挪作他用。Java 中的关键字全部是小写字母

## 保留字

Java 语言中的保留字只有两个`goto`和`const`

- `goto` 在其他语言中叫做"无限跳转"语句，在 Java 语言中不再使用 goto 语句，因为"无限跳转"语句会破坏程序结构。
  在 Java 语言中 goto 的替换语句可以通过`break`、`continue`、`return`实现"有限跳转"

- `const` 在其他语言中是声明常量关键字，在 Java 语言中声明常量使用`public static final`方式声明

## 变量

变量和常量是构成表达式的重要部分。变量根据作用域不同分为，成员变量和局部变量

成员变量是在类体中，而在方法外作用域是整个类，如果没有初始赋值，系统会为它分配一个默认值，每一种数据类型都有
默认值，int 类型默认值是`0`

局部变量是在代码块中声明的变量，与成员变量不同的是，局部变量在使用之前必须显示地初始化

## 常量

常量事实上是哪些内容不能被修改的变量，在声明常量同时要赋予一个初始值，常量一旦初始化就不能被修改

```
final 数据类型 变量名 = 初始值;
```

> final 关键字表示最终

常量有三种类型，静态常量、成员常量、局部常量

静态常量在 final 之前用`public static` 修饰，其作用域是全局的，不需要创建对象就可以访问他，如

```java
public class HelloWorld {
  public static final double PI = 3.14;
}
HelloWorld.PI
```
