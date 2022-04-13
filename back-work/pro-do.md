# 预处理

C++ 提供了预处理功能主要有以下 4 种，宏定义、文件包含、条件编译和布局控制

## 宏定义命令

简单的宏定义的声明格式

```
#define 宏名 字符串
```

例如

```
#define PI 3.1415926
```

带参数的宏定义的声明格式

```
#define 宏(参数列表) 宏
```

## 条件编译

一般情况下，源程序中所有行的语句都参数编译，但是有时程序员希望其中一部分内容，只在满足
一定条件时才进行编译，这就是对一部分内容指定编译的条件

```
#ifdef 标识符
  程序段1
#else
  程序段2
#endif
```

```c++
#include<iostream>
using namespace std;

#define _DEBUG_

int main() {
  int x = 10;

  #ifdef _DEBUG_
    cout<< "File:" << __FILE__ << ",Line:" << __LINE__ << ",x:" << x <<endl;
  #else
    printf("x = %d\n", x)
  #endif

  return 0;
}
```

## 小结

学习 C++，既要会利用 C++进行面向过程的结构化程序设计，也要会利用 C++进行面向对象
的程序设计
