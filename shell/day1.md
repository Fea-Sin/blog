## Shell 变量

定义变量时，变量名不加美元符号

> 注意，变量名和等号之间不能有空格，这可能与你熟悉的所有编程语言不一样

```bash
your_name="runoob.com"
```

除了显式地直接赋值，还可以用语法给变量赋值

```bash
# 将/etc 目录下的文件名循环出来

for file in `ls /etc`
```

### 使用变量

使用一个定义过的变量，只要在变量名前加美元符号即可

```bash
your_name="hello"

echo $your_name
echo ${your_name}
```

变量名外的花括号是可选的，推荐所有的变量都加上花括号，这是个好的编程习惯

### 只读变量

使用`readonly`命令可以将变量定义为只读变量，只读变量的值不能被改变

```bash
#!/bin/bash

myUrl="https://www.google.com"

readonly myUrl
```

### 删除变量

使用`unset`命令可以删除变量

```bash
unset variable_name
```

变量删除后不能再次使用，`unset`命令不能删除只读变量

### 变量类型

运行 shell 时，会同时存在三种变量

- **局部变量**，局部变量在脚本或命令中定义，仅在当前 shell 实例中有效，其他 shell 启动的程序不能访问局部变量

- **环境变量**，所有的程序，包括 shell 启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行，必要的
  时候 shell 脚本也可以定义环境变量

- **shell 变量**，shell 变量是由 shell 程序设置的特殊变量，shell 变量中有一部分是环境变量，有一部分是局部变量，这些
  变量保证了 shell 的正常运行

## Shell 字符串

字符串是 shell 编程中最常用最有用的数据类型，字符串可以用双引号、单引号、也可以不用引号

#### 双引号

- 双引号里可以有变量

- 双引号里可以出现转译字符

#### 单引号

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的

- 单引号字符串不能出现单独一个的单引号（对单引号使用转译后也不行），但可成对出现，作为字符串拼接使用

```bash
your_name="runoob"

str="Hello, I know you are \"${your_name}\" !"

# Hello, I know you are "runoob"
```

#### 获取字符串长度

```bash
string="abcd"

echo ${#string}

# 4
```

#### 提取子字符串

```bash
string="runoob is a great site"
echo ${string:1:4}

# unoo
```

> 注意，第一个字符的索引值为`0`

#### 查找子字符串

```bash
# 查找字符`i`或`o`的位置

string="runoob is a great site"

echo `expr index "${string} io"`
```

## Shell 数组

bash 支持一维数组（不支持多维数组），并且没有限定数组的大小

类似于 C 语言，数组元素的下标由`0`开始编号，获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值
应大于等于`0`

### 定义数组

在 Shell 中，用括号来表示数组，数组元素用空格符号分割开

```bash
数组名=(值1 值2 ... 值n)
```

或者

```bash
array_name=(
  value0
  value1
  value2
)
```

### 读取数组

```bash
${数组名[下标]}
```

```bash
valuen=${array_name[n]}
```

### 获取数组的长度

```bash
length=${#array_name[@]}

# 或者

length=${#array_name[*]}
```

## Shell 注释
