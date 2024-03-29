# 结构体、公用体、枚举

结构体的声明方法

```
struct 结构体 {
  数据类型 成员名;
  ...
  数据类型 成员名;
}
```

声明一个结构体类型 student

```c++
struct student {
  int num;
  char name[20];
  int age;
}

struct student stu1;
stu1.num = 1;
```

引用结构体变量中成员的一般方式为

```
结构体变量名.成员名
```

> 指针也可以应用于结构体

```c++
student *stu3 = &stu1;
```

## 共用体

共用体，用关键字`union`来定义，它是一种特殊的类，在一个共用体里可以定义多种不同的
数据类型，这些数据共享一段内存，在不同的时间里保存不同的数据类型和长度的变量，以达到
节省空间的目的，但同一时间只能存储其中一个成员变量的值

共用体的声明方式

```
union 共用体类型 {
  数据类型 成员名;
  ...
  数据类型 成员名;
} 变量名
```

## 枚举

在实际问题中，有些变量的取值被限定在一个有限的范围内，如，一个星期只有 7 天，如果把这些量
说明为整型、字符型或其它类型显然不妥当。为此，C 语言提供了一种称为`枚举`的类型，枚举类型
在 C++中同样适用。

在`枚举`类型的定义中列举出所有可能的取值，用来说明该`枚举`类型的变量取值不能超过定义的
范围。

枚举类型是一种基本数据类型，而不是一种构造类型，因为它不能再分解为任何其他基本类型

枚举的声明方式

```
enum 枚举类型名 { 枚举常量表列 }
```

```c++
enum weekday { sun, mou, tue, wed, thu, fri, sat };

enum weekday a, b, c;
```

或者

```c++
enum weekday { sun, mou, tue, wed, thu, fri, sat } a, b, c;
```

枚举值是常量，不是变量，不能在程序中用赋值语句再对它赋值

枚举元素不是字符常量也不是字符串常量，使用时不要加单、双引号

## 结构体、共用体在内存单元占用字节数的计算

一般 64 位机器上各个数据类型所占的存储空间

- `char` 8bit = 1byte
- `short` 16bit = 2byte
- `int` 32bit = 4byte
- `long` 64bit = 8byte
- `float` 32bit = 4byte
- `double` 64bit = 8byte
- `long long` 64bit = 8byte

其中，`long`类型在 32 位机器上只占 4byte，其他类型在 32 位机器和 64 位机器都是占
同样的大小空间

### `union`共用体的字节计算

```c++
union A {
  int a[5];
  char b;
  double c;
}
```

union 中变量共用内存，共用体内变量的默认内存对齐方式，必须以最长的`double(8Byte)`对其
