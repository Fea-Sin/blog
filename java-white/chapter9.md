# 抽象类与接口

在 Java 中具有抽象方法的类称为"抽象类"

在 Java 中抽象类和抽象方法的修饰符是`abstract`

抽象方法只有方法的声明，没有方法的实现，即没有打括号部分。如果一个方法被声明为抽象的，那么这个类
也必须声明为抽象的
设计抽象方法的目的就是让子类来实现

> 抽象类不能被实例化，只有具体类才能被实例化

```java
public abstract void onDraw();
```

## 接口

比抽象类更加抽象的是接口，在接口中所有方法都是抽象的。在 Java 中接口的声明使用的关键字是`interface`

```java
public interface Figure {
  String name = "几何图形";

  void onDraw();
}
```

某个类实现接口时，要在声明时使用`implements`关键字，当实现多个接口之间用逗号分隔，实现接口时要实现
接口中声明的所有方法

> 接口与抽象类一样都不能被实例化

## 接口与多继承

在 C++语言中一个类可以继承多个父类，但这会有潜在风险，如果两个父类有相同的方法，那么子类如何继承哪一个方法呢，
这就是 C++多继承所导致的冲突问题

在 Java 中只允许继承一个类，但可以实现多个接口，通过实现多个接口方式满足多继承的设计需求。
多个接口即便有相同的方法，它们也都是抽象的，子类实现它们不会有冲突

## 接口继承

Java 语言中允许接口和接口之间继承，由于接口中的方法都是抽象方法，所以继承之后不需要做什么，因此接口之间的
继承要比类之间的继承简单的多

## Java8 新特性，接口中默认方法和静态方法

尽管 Java 语言中接口已经非常优秀了，但还有不足之处

- 不能可选实现方法，接口的方法全部是抽象的，实现接口时必须全部实现接口中方法，那怕有些方法并不需要
  也必须实现

- 没有静态方法

```java
public interface InterfaceA {
  void methodA();

  // 默认方法
  default int methodC() {
    return 0;
  }

  // 静态方法

  static double methodE() {
    return 0.0;
  }
}
```

## 抽象类和接口区别

- 接口支持多继承，而抽象类、具体类只能继承一个父类

- 接口中不能有实例成员变量，接口所有声明的成员变量全部是静态常量

- 接口中没有包含构造方法，由于没有实例成员变量，也就不需要构造方法了。抽象类中可以有实例成员变量，也需要构造方法