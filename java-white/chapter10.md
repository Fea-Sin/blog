# 异常处理

为增强程序的健壮性，计算机程序的编写也需要考虑这些异常情况，Java 语言提供了异常处理功能

```
              ----->  Error
Throwable
              ----->  Exception
```

## Throwable 类

所有的异常类都直接或间接继承于 java.lang.Throwable 类，在 Throwable 类有几个重要的方法

- String getMessage()，获得发生异常的详细消息

- void printStackTrace()，打印异常堆栈跟踪信息
  堆栈跟踪是方法调用的轨迹，它包含了程序执行过程中方法调用的顺序和所在源代码行号
  堆栈跟宗信息从下往上是方法调用顺序

- String toString()，获得异常对象的描述

try-catch 可以捕获异常，在 catch 中有一个 Throwable 对象，throwable 对象是在系统在程序发生异常时创建

### Error

Error 是程序无法恢复的严重错误，程序员根本无能为力，只能让程序终止，例如 JVM 内部错误、内存溢出和资源耗尽等

### Exception

Exception 是程序可以恢复的异常，它是程序员所能掌控的。例如除零异常、空指针访问、网络连接中断和读取不存在的文件等。
本章所讨论的就是对 Exception 及其子类的异常处理

## 运行异常

运行异常是继承 RuntimeException 类的直接或间接子类。运行异常往往是程序员所犯错误导致的，健壮的程序
不应该发生运行时异常。

编译器不检查这类异常是否处理，一旦运行时异常就会导致程序的终止。ArithmeticException 异常属于 RuntimeException 异常

对于运行时异常通常不采用抛出或捕获处理方式，而是应该提前预判，防止这种异常发生，做到未雨绸缪

```java
public static int divide(int number, int divisor) {
  if (divisor != 0) {
    return number / divisor;
  }
  return 0;
}
```

## 受检查异常

受检查异常是除 RuntimeException 以外的异常类，它们的共同特点是，编译器会检查这类异常是否进行了
处理，即要么捕获(try-catch 语句)，要么不抛出异常(通过在方法后声明 throws)，否则会发生编译错误

## 捕获异常

对待受检查异常时，当前方法有能力解决，则捕获异常进行处理，没有能力解决，则抛出给上层调用方法处理，如果
上层调用方法还无力解决，则继续抛出给它的上层调用方法，异常就是这样向上传递直到有方法处理它，如果所有方法都
没有处理该异常，那么 JVM 会终止程序运行

## try-catch 语句

基本语法

```java
try {

} catch(Throwable e) {

}
```

try 代码块中应该包含执行过程中可能会发生异常的语句

静态方法、实例方法和构造方法都可以声明抛出异常，凡是抛出异常的方法都可以通过 try-catch 进行捕获

## 多 catch 代码块

如果 try 代码块中有很多语句会发生异常，而且发生的异常种类又很多，那么可以在 try 后面跟多个 catch 代码块

```java
try {

} catch(Throwable e) {

} catch(Throwable e) {

}
```

在多个 catch 代码情况下，当一个 catch 代码块捕获到一个异常时，其他的 catch 代码块就不再进行匹配。
捕获异常顺序与 catch 代码块的顺序有关，一般先捕获子类，后捕获父类
没有父子关系的异常，可以随意放置位置

## try-catch 语句嵌套

try-catch 不仅可以嵌套在 try 代码块中，还可以嵌套在 catch 代码块或 finally 代码块，try-catch
嵌套会使流程变得复杂，如果能用多 catch 捕获的异常，尽量不要使用 try-catch 嵌套

## 多重捕获

多 catch 代码块客观上提高了程序的健壮性，如果多重捕获之后处理方法相同，可以合并处理

```java
try {

} catch(IOException | ParseException) {
  // 调用 methodA()
}
```

## 释放资源

有时在 try-catch 语句中会占有一些非 Java 资源，如打开问价、网络连接、打开数据库连接和使用数据库结果集等，
这些资源不能被 JVM 的垃圾收集器回收，需要程序员释放

### finally 块

try-catch 语句后面可以跟一个 finally 代码块

无论 try 正常结束还是 catch 异常结束都会执行 finally 代码块

## throw 与显示抛异常

Java 异常相关的关键字有两个非常相似，throws 和 throw，throws 用于方法后声明抛出异常，
throw 关键字用来人工引发异常。当异常发生时，系统一个异常对象，将其抛出。但也可通过 throw 语句显式
抛出异常

```
throw Throwable;
```

显式抛出异常目的有很多，例如不想某些异常传给上层调用者，可以在捕获之后重新显示抛出另外一个异常(通常是自定义异常)
给调用者

throw 显式抛出异常与系统生成并抛出的异常，在处理方式上没有区别，这两种方法，要么捕获自己处理，
要么抛出给上层调用者
