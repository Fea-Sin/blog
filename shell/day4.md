## 输出重定向

重定向一般通过在命令间插入特定的符号来实现

```bash
command > file
```

上面这个命令执行 command 然后将输出的内容存入 file

注意任何 file 内的已经存在的内容将被新的内容替代，如果要将新的内容添加在文件末尾，请使用`>>`操作符

实例

```bash
who > users
```

执行后，并没有在终端输出信息，这是因为输出已被从默认的标准输出设备终端重定向到指定的文件

你可以使用`cat`命令查看文件内容

```bash
cat users
```

输出重定向会覆盖文件内容，入股不希望文件内容被覆盖，可以使用`>>`追加到文件末尾

## 输入重定向

和输出重定向一样，也可以从文件获取输入

```bash
command < file
```

这样，本来需要从键盘获取输入的命令会转移到文件读取内容

```
wc -l < users
```

**同时替换输入和输出**

```bash
command < infile > outfile
```

执行 command，从文件 infile 读取内容，然后写入到 outfile 文件中

## 重定向深入讲解

一般情况下，每个 Unix/Linux 命令运行都会打开三个文件

- 标准输入文件(stdin): stdin 的文件描述符为 0，Unix 程序默认从 stdin 读取数据

- 标准输出文件(stdout): stdout 的文件描述符为 1，Unix 程序默认向 stdout 输出数据

- 标准错误文件(stderr): stderr 的文件秒睡符为 2，Unix 程序会向 stderr 流中写入错误信息

如果希望 stderr 重定向到 file，可以这样写

```bash
command 2> file
```

追加到 file

```bash
command 2>> file
```

如果希望将 stdout 和 stderr 合并后重定向到 file

```bash
command > file 2>&1

# or 追加
command >> file 2>&1
```

## shell 文件包含

和其他语言一样，shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件

shell 文件包含的语法格式如下

```bash
# `.`和文件名中间有一空格
. filename

# or

source filename
```
