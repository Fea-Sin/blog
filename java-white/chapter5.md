# 字符串

由字符组成的一串字符虚列，称为字符串。Java 中的字符串是由双引号扩起来的多个字符

Java 中的字符采用 Unicode 编码，所以 Java 字符串可以包含中文等亚洲字符

单个字符如果用双引号扩起来，那它表示的是字符串，而不是字符了

> 字符串还有一个极端情况，""表示空字符串，双引号中没有任何内容，空字符串不是 null，
> 空字符串是分配了内存空间，而 null 表示没有分配内存空间

Java SE 提供了三个字符串类，String, StringBuffer, StringBuilder

String 是不可变字符串，StringBuffer，StringBuilder 是可变字符串

Java 中很多类，每一个类又有很多方法和变量，通过查看 Java API 文档能够知道这些类、方法、变量如何使用

## 不可变字符串

很多计算语言都提供了两种字符串，即不可变字符串和可变字符串，它们区别在于当字符串进行拼接等修改操作时，不可变字符串
会创建新的字符串对象，而可变字符串不会创建新对象

java.lang 包中提供了很多 Java 基础类，包括 Object、Class、String、Math 等基类

在使用 java.lang 包中的类时不需要引入该包，因为它是由解释器自动引入的

## 字符串池

```java
String s1  = new String("hello");
String s2 = new String("hello");

String s3 = "hello";
String s4 = "hello";

s1 == s2; // false
s3 == s4; // true
```

Java 中的不可变字符串 String 常量，采用字符串池管理技术，字符串池是一种字符串驻留技术。
会查找字符串池中对应字符串常量，如果已经存在把引用赋值给变量，否则创建字符串对象，并放入池中

Java 中所有对象都有一个 toString()方法，该方法可以将对象转换为字符串，拼接过程会调用该对对象的 toString()方法，
将该对象转换为字符串后再进行拼接

## 字符串查找

在给定的字符串中查找字符或字符串是比较常见的操作，在 String 类中提供了 indexOf 和 lastIndexOf 方法用于查找字符或
字符串，返回值是查找的字符或字符串所在的位置，-1 表示没有找到，这两个方法有多个重载版本

字符串本质上是字符数组，因此它也有索引，索引从零开始。String 的 charAt(int index)方法可以返回索引 index 所在位置的字符

## 字符串比较

字符串比较是常见的操作，包括比较相等、比较大小、比较前缀和后缀串等

- `boolean equals()` 比较两个字符串中内容是否相等

```java
String s1 = new String("hello");
String s2 = new String("hello");

s1.equals(s2);
```

## 字符串截取

String substring(int beginIndex, int endIndex);

## 可变字符串

可变字符串在追加、删除、修改、插入和拼接等操作不会产生新的对象。
字符串缓冲区

StringBuffer 是线程安全的，它的方法是支持线程同步，线程同步会操作串行顺序执行，在单线程环境下
会影响效率。StringBuilder 是 StringBuffer 单线程版本

> 字符串长度和字符串缓冲区容量区别。字符串长度是指字符串缓冲区中目前所包含字符串长度，通过 length()获得，
> 字符串缓冲区容量是缓冲区所能容纳的最大字符数，通过 capacity()获得。当所容纳的字符超过这个长度时，字符串
> 缓冲区自动扩充容量，但是以牺牲性能为代价的扩容

> 线程同步是一个多线程概念，就是当多个线程访问一个方法时，只能由一个优先级别高的线程先访问，在访问期间会锁定
> 该方法，其他线程只能等到它访问完成释放锁，才能访问

## 字符串插入、删除、替换

- `StringBuilder insert(int offset, String str)` 在字符串缓冲区中索引为 offset 的字符位置之前
  插入 str，insert 有很多重载方法，可以插入任何类型数据

StringBuffer delete(int start, int end);

StringBuffer replace(int start, int end, String str);
