# Shell

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种
设计语言

Shell 是一种应用程序，这个应用程序提供了一个界面，用户通过这个界面的访问操作系统内核的服务

Shell 脚本(shell script)是一种为 Shell 编写的脚本程序

shell 和 shell script 是两个不同的概念

## `#!`

`#!`告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序

`#!`是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行

## 运行脚本的两种方法

**作为可执行程序**

使脚本具有执行权限

```bash
chmod +x ./test.sh

# 执行脚本
./test.sh
```

> 注意，一定要写成`./test.sh`，而不是`test.sh`，运行其他二进制的程序也一样，linux 系统会去 PATH 里
> 寻找有没有叫`test.sh`的，而只有`/bin`，`/sbin`，`/usr/bin`，`/usr/sbin`等在 PATH 里。你的当前目录
> 通常不在 PATH 里，要用`./test.sh`告诉系统就在当前目录找

**作为解释器参数**

这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名

```bash
/bin/sh test.sh
```
