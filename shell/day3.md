## shell printf 命令

printf 命令模仿 C 程序库里的 printf()程序

printf 由 POSIX 标准所定义，因此使用 printf 的脚本比使用 echo 移植性更好

printf 使用引用文本或空格分割参数，外面可以在 printf 中使用格式化字符串，还可以
制定字符串的宽度、左右对齐方式等。默认 printf 不会像`echo`自动添加换行符，我们可以
手动添加`\n`

### 语法

```bash
printf format-string [arguments...]
```

- format-string: 格式控制字符串
- arguments: 参数列表

```bash
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234
```

`%s` 输出一个字符串
`%d` 整型输出
`%c` 输出一个字符
`%f` 输出实数，以小数形式输出

`%-10s` 指一个宽度为 10 个字符，`-`表示左对齐，没有侧表示右对齐，任何字符都会被
显示在 10 个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来

`%-4.2f` 指格式化为小数，其中`.2`指保留 2 位小数

## printf 的转义序列

`\f` 换页

`\n` 换行

`\r` 回车

`\t` 水平制表符

`\v` 垂直制表符

## shell 流程控制

和 Java、PHP 等语言不一样，sh 的流程控制不可为空。如果 else 分支没有
语句执行，就不要写这 else

## if 语句语法

```bash
if condition
then
  command1
  command2
  ...
  commandN
fi
```

写成一行，适用于终端

```bash
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi
```

## if else 语法

```bash
if condition
then
  command
else
  command
fi
```

## if else-if else

```bash
if conditon1
then
  command
elif condition
then
  command
else
  command
fi
```

## for 循环

与其他编程语言类似，shell 支持 for 循环

for 循环一般格式为

```bash
for var in item1 item2 ... timeN
do
  command1
  command2
  ...
  commandN
done
```

写成一行

```bash
for var in item1 itme2 ... itmeN; do command1; command2; done;
```

变量获取列表中的当前值，for 循环执行一次所有命令，命令可以为任何有效的 shell 命令和语句

```bash
for loop in 1 2 3 4 5
do
  echo "The value is: ${loop}"
done

# The value is: 1
# ...
# The value is: 5
```

## while 语句

while 循环用于不断执行一系列命令，也用于从文件中读取数据

```bash
while condition
do
  command
done
```

实例

```bash
#!/bin/bash

int=1

while (( $int<=5 ))
do
  echo $int
  let "int++"
done
```

## until 循环

until 循环执行一系列命令直至条件为 true 时停止
until 循环与 while 循环在处理方式上刚好相反

```bash
until condition
do
  command
done
```

condition 一般为条件表达式，如果返回值为 false，则继续执行循环体内的语句，否则跳出循环

```bash
a=0

until [ ! $a -lt 10 ]
do
  echo $a
  a=`expr $a + 1`
done
```

## shell 函数

linux shell 用户可以定义函数，然后在 shell 脚本中可以随便调用

- 可以直接`fun()`定义，不带任何参数

- 参数返回，可以显式加`return`返回，如果不加，将以最后一条命令运行结果，作为返回值

实例

```
demoFun() {
  echo "这是我的第一个shell函数"
}

echo "-----函数执行开始--------"
demoFun
echo "-----函数执行结束--------"
```

## 函数参数

调用函数时可以向其传递参数，在函数内部，通过`$n`的形式来获取参数的值。`$1`表示第一个参数，
`$2`为第二个参数...

实例

```bash
funWithParam() {
  echo "第一个参数 $1"
  echo "第一个参数 $2"
  echo "参数总数有 $# 个"
  echo "所有参数 $*"
}

funWithParam 5 89
# 第一个参数 5
# 第一个参数 89
# 参数总数有 2 个
# 所有参数 5 89
```
