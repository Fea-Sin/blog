# 控制语句

程序设计中的控制语句有三种，顺序、分支、循环语句

Java 中的控制语句有以下几类

分支语句: if, switch

循环语句: while, do-while, for

跳转语句: break, continue, return, throw

## switch 语句

switch 语句语法结构

```
switch(表达式) {
  case 值1:
    语句组1
  case 值2:
    语句组2
    ...
  case 判断值n:
    语句组n
  default:
    语句组n+1
}
```

switch 语句中`表达式`计算结果只能是 int、byte、short、char 类型，不能是 long 更不能是其他的类型。
每个 case 后面只能跟一个 int、byte、short、char 类型常量，default 语句可以省略

当程序执行到 switch 语句时，先计算条件表达式的值，假设值为 A，然后拿 A 与第 1 个 case 语句中的值 1 进行
匹配，如果匹配则执行语句组 1，语句组执行完成后不跳出 switch，只有遇到 break 才跳出 switch。如果 A 没有与
第 1 个 case 语句匹配，则与第 2 个 case 语句进行匹配，直到执行语句组 n。如果所有的 case 语句没有执行，
就执行 default 的语句组 n+1，这时才跳出 switch

当把 case7 和 case6 当成一种情况考虑时，case7 和 case6 都执行相同的代码

```
case7:
case6:
  grade = "中";
  break;
```

## 循环语句

Java 支持 3 种循环构造类型，while、do-while 和 for

for 和 while 是在执行循环体之前测试循环条件，而 do-while 是在执行循环体之后测试循环条件
这意味着 for 和 while 循环可能连一次循环体都未执行，而 do-while 将至少执行一次循环体

### while 语句

while 语句没有初始化语句，循环次数是不可知的，只要循环条件满足，循环就会一直进行下去

while 循环条件语句只能写一个表达式，而且是一个布尔表达式，如果循环体中需要循环变量，就必须在 while 语句之前
对循环变量进行初始化，然后在循环体内部必须通过语句更改循环变量的值，否则将会发生死循环

### do-while 循环

do-while 循环没有初始化语句，循环次数是不可知的，不管循环条件是否满足，都会先执行一次循环体，然后再判断循环条件，
如果条件满足则执行循环体，不满足则停止循环

### for 语句

for 语句是应用最广泛、功能最强的一种循环语句

```
for (初始化; 循环条件; 迭代) {
  语句组
}
```

for 语句首先执行初始化语句，它的作用是初始化循环变量和其他变量，然后程序会判断循环条件是否满足，
如果满足，则继续执行循环体并计算迭代语句，之后再判断循环条件。如此反复，直到判断循环条件不满足时跳出循环

```

             开始
              ｜
        执行初始化语句
              ｜ <------------------------------------
              ｜                                     ｜
              ｜         yes
        循环条件是否满足  ------> 执行循环体 ------> 计算迭代语句
              ｜
              ｜no
              ｜
             结束
```

> 初始化、循环条件和迭代部分，都可以为空语句，但分号不能省略，三者均为空时，相当于一个无限循环

```java
for (;;) {
  ...
}
```

> 在初始化部分和迭代部分，可以使用逗号语句来进行多个操作
> 逗号语句是用逗号分隔的语句序列

```java
int x;
int y;

for (x=0, y=10; x<y; x++, y--) {
  System.out.printf("(x,y)=(%d, %d)", x, y);
  System.out.println();
}
```

### for-each 语句

```java
int[] numbers = { 43, 32, 53, 54, 75, 7, 10 }

for (int item : numbers) {
  System.out.println("Count is:", item)
}
```

## 跳转语句

跳转语句能改变程序的执行顺序，可以实现程序的跳转。Java 有 4 种跳转语句
break, continue, throw, return

### break

break 语句的作用是强制退出循环体，不再执行循环体中剩余的语句

在循环体中使用 break 语句有两种方式，带有标签和不带标签

不带标签的 break 语句使程序跳出所在层的循环体，而带标签的 break 语句使程序跳出标签指示的循环体

**break 配合标签使用**

```java
label1: for(int x=0; x<5; x++) {
  for (int y=5; y>0; y--) {
    if (y == x) {
      break label1;
    }
    System.out.printf("(x,y)=(%d, %d)", x, y)
  }
}
System.out.println("Game Over!")
```

> 添加标签对于多层嵌套循环是很有必要的，适当使用可以提高程序的执行效率

### continue 语句

continue 语句用来结束本次循环，跳过循环体中尚未执行的语句，接着进行条件判断，以决定是否继续循环

对于 for 语句在终止条件的判断前，还要先执行迭代语句

在循环体中使用 continue 语句有两种方式，带标签和不带标签

默认情况下，continue 只会跳出最近的内循环，如果要跳到外循环，可以为外循环添加一个标签

```java
label1: for(int x=0; x<5; x++) {
  for (int y=5; y>0; y--) {
    if (y == x) {
      continue label1;
      System.out.printf("(x,y)=(%d, %d)", x, y)
    }
  }
}
System.out.println("Game Over!");
```
