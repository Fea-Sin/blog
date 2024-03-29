# for 语句

for 语句用于创建一个循环，它包含了三个可选的表达式，这三个表达式被包围在圆括号之中，使用分号
分隔，后跟一个用于在循环中执行的语句（通常是一个块语句）

## 语法

```
for ([initialization]; [condition]; [final-expression])
  statement
```

### initialization

一个表达式（包含赋值语句）或者变量声明。典型地被用于初始化一个计数器。该表达式可以使用
`var`或`let`关键字声明新的变量，使用`var`声明的变量不是该循环的局部变量，而是与`for`循环
处在同样的作用域中。用`let`声明的变量是语句的布局变量。改表达式的结果无意义。

### condition

一个条件表达式被用于确定每一次循环是否被执行。如果该表达式的结果为`true`，`statement`将
被执行。这个表达式是可选的。如果被忽略，那么就被认为永远为真。如果计算结果为假，那么执行流程
将被跳转到`for`语句后面的第一条语句（for 循环执行结束）。

### final-expression

每次循环的最后都要执行的表达式。执行时机是在下一次`condition`的计算之前。通常被用于
更新或者递增计数器变量

### statement

只要`condition`的结果为`true`就会被执行的语句。要在循环体内执行多条语句，使用一个
块语句`{ ... }`来包含要执行的语句。没有任何语句要执行，使用一个空语句`;`
