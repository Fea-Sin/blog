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
