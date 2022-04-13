# 数据类型

Java 语言的数据类型分为，基本类型和引用类型

## 基本数据类型

基本类型表示简单的数据，基本类型分为 4 个大类，共 8 种数据类型

- 整数类型，byte、shot、int、long

- 浮点类型，float 和 double

- 字符类型，char

- 布尔类型，boolean

### 整型类型

Java 中整数都是有符号，与 C 不同没有无符号的整数类型。

- byte，1 个字节(8 位)，`-128~127`

- short，2 个字节(16 位)，`-2(15次方)～2(15次方)-1`

- int，4 个字节(32 位)，`-2(31次方)~2(31次方)-1`

- long，8 个字节(64 位)，`-2(63次方)~2(63次方)-1`

> Java 语言的整数类型默认是 int 类型

### 浮点类型

浮点类型主要用来存储小数数值，也可以用来存储较大的整数

- 浮点数 float，4 个字节(32 位)

- 双精度浮点数 double，8 个字节(64 位)

Java 语言的浮点类型默认是`double`类型

如果想表示 float 类型，则需要在数值后面加 f 或 F

如果为一个整数变量赋值，使用二进制、八进制、十六进制表示

- 二进制，以 0b 或 0B 为前缀

- 八进制，以 0 为前缀

- 十六进制，以 0x 或 0X 为前缀

如果用指数表示数值，需要使用大写或小写的 e 表示幂，e2 表示 10(2 次方)

### 字符类型

Java 中 char 声明字符类型，Java 中的字符常量必须用单引号扩起来。Java 字符采用双字节 Unicode 编码，占用两个字节(16 位)

> 字符类型也属于数值类型，可以与 int 等数值类型进行数学计算或进行转换

双字节 Unicode 存储范围

```
\u0000 ~ \uFFFF，char 类型取值范围0-2(16次方)-1
```

> 在 C 语言中布尔类型是数值类型，它有两个取值 1 和 0。而在 Java 中的布尔类型取值不能用 1 和 0 替代
> 也不属于数值类型，不能与 int 等数值类型之间进行数学计算或类型转化

## 数值类型相互转换

数据类型之间是否可以相互转换呢，数据类型的转换情况比较复杂。基本数据类型中数值类型之间可以相互转换，布尔
类型不能与他们之间转换。

转换分为，自动类型转换，强制类型转换

### 自动类型转换

自动类型转换就是需要类型之间转换是自动的，不需要采取其他手段，总的原则是小范围数据类型可以自动转换为大范围数据类型

> char 类型比较特殊，char 自动转换为 int、long、float、double，byte 和 short 不能自动转换为 char，
> 而且 char 也不能自动转换为 byte 和 short

自动类型转换不仅发生在赋值过程中，在进行数学计算也会自动类型转换，在运算中往往是先将数据类型转换为同一类型，
然后在进行计算

### 强制类型转换

在数据类型转换过程中，除了需要自动类型转换外，有时还需要强制类型转换，强制类型转换是在变量或常量之前加上"目标类型"，
强制类型转换主要用于大宽度类型转换为小宽度类型情况

```java
int i = 10;
byte b = (byte)i;
```

## 引用数据类型

在 Java 中除了 8 种基本数据类型外，其他数据类型全部都是引用(reference)数据类型，引用数据类型
用了表示复杂数据类型

> Java 中的引用类型，相当于 C 等语言中指针(pointer)类型，引用事实上就是指针，是指向一个对象
> 的内存地址。引用类型变量中保持的是指向对象的内存地址。很多资料上提到 Java 不支持指针，事实上是
> 不支持指针计算，而指针类型还是保留了下来，就是引用类型

## 预算符

Java 语言中的运算符(也称为操作符)在风格上和功能上都与 C 和 C++极为相似，主要的运算符包括，算术运算符、
关系运算符、逻辑运算符、位运算符和其他运算符

### 算术运算符

Java 中的算术运算符主要用来组织数值类型数据的算术运算，按照参加运算的操作数的不同可以分为一元运算符和二元
运算符

`a++或a--` 是在表达式运算完后，再给 a 加一或减一

`++a或--a` 是先给 a 加一或减一，然后再进行表达式运算

### 关系远算符

关系远算是比较两个表达式大小的运算，它的结果是布尔类型数据，即 true 或 false

> `==`和`!=` 可以应用于基本数据类型和引用类型，当用于引用类型比较时，比较的是两个应用是否指向同一个对象，
> 但实际开发过程情况下啊，只是比较对象的内容是否相等，不需要比较是否为同一个对象

### 逻辑运算符

逻辑运算符是对布尔型变量进行运算，其结果也是布尔类型

> 短路与(&&)和短路或(||)能够采用最优化的计算方式，从而提高效率，在实际编程时，应该优先考虑使用

### 位运算符

位运算符是以二进制位(bit)为单位进行运算的，操作数和结果都是整型数据。

`～x` 将 x 的值按位取反

`x&y` x 与 y 位进行位与运算

`x|y` x 与 y 位进行位或运算

`x^y` x 与 y 位进行位异或运算

`>>` 有符号右移 `x>>a` x 右移 a 位，高位采用符号位补位

`<<` 左移 `x<<a` x 左移 a 位，低位用 0 补位

`>>>` 无符号右移 `x>>>a` x 右移 a 位，高位用 0 补位

> 无符号右移运算仅被允许用在 int 和 long 整数类型，如果用于 short 或 byte 数据
> 则数据在位移之前，转换为 int 类型后再进行位移计算

有符号右移 n 位，相当于操作数除以(2 的 n 次方)
左位移 n 位，相当于操作数乘以(2 的 n 次方)

## 其他运算符

`.`引用号，对象调用实例变量或实例方法的操作符，也是类调用静态变量或静态方法的操作符

`instanceof` 判断某个对象是否为属于某个类

`new` 对象内存分配运算符

`->` Java8 新增的，用来声明 Lambda 表达式

`::` Java8 新增的，用于 Lambda 表达式中方法的引用

## 运算符优先级

从高到低

算术运算符，位运算符，关系运算符，逻辑运算符，赋值运算符