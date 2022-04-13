# 文件管理与 I/O 流

程序经常需要访问文件和目录，读取文件信息或写入信息到文件，在 Java 语言中对文件读写是通过 I/O 流技术实现的

## 文件管理

File 类表示一个与平台无关的文件或目录。File 类名很有欺骗性，初学者认为 File 对象只是一个文件，
但也可能是一个目录

路径中会用到路径分隔符，路径分隔符在不同的平台上是有区别的，UNIX、Linux 和 macOS 使用正斜杠`/`，
而 Windows 下使用反斜杠`\`。Java 支持两种写法，但时反斜杠`\`属于特殊字符，需要加转义符

```
C:/Users/a.java
```

```
C:\\Users\\a.java
```

## I/O 流概述

Java 将数据的输入输出操作当做流来处理，流是一组有序的数据序列。从数据源中读取数据是输入流，
将数据写入到目的地是输出流

数据输入的数据源有多种形式，如文件、网络和键盘，键盘是默认的标准输入设备

数据输出的目的地有多种形式，如文件、网络和控制台，控制台是默认的标准的输出设备

## 流类继承层次

以字节为单位的流称为字节流，以字符为单位的流称为字符流。Java SE 提供了 4 个顶级抽象类

两个字节流抽象类，InputStream 和 OutputStream
两个字符流抽象类，Reader 和 Writer

流所占用的资源，不能通过 JVM 的垃圾收集器回收，需要程序员自己释放。一种方式是可以在 finally 代码
块调用 close()方法关闭流，另一种方法通过自动资源管理技术管理这些流

> 过滤流实现了装饰器设计模式，这种设计模式能够在运行时扩充一个类的功能

## 字符流

掌握字符流的 API 先要熟悉它的两个抽象类，Reader 和 Writer

Reader 是字符输入流的根类，Writer 是字节输出流的根类

## 字节流转换字符流

有时需要将字节流转换为字符流， InputStreamReader 和 OutputStreamWriter

InputSreamReader 构造方法如下

- InputStreamReader(InputStream in) 将字节流 in 转换为字符流，字符使用默认字符集

- InputStreamReader(InputStream in, String charsetName) 将字节流 in 转换为字符流对象
  charsetName 指定字符流的字符集，字符集主要有 `US-ASCII`, `ISO-8859-1`, `UTF-8`, `UTF-16`
