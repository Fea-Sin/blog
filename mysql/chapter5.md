# 数据类型和运算符

## MySQL 数据类型介绍

MySQL 支持多种数据类型，主要有数值类型、日期/时间类型和字符串类型

- 数值数据类型，包括整数类型 `TINYINT`、`SMALLINT`、`MEDIUMINT`、`INT`、`BIGINT`浮点小数数据类型
  `FLOAT`和`DOUBLE`，定点小数类型`DECIMAL`

- 日期/时间类型，包括`YEAR`、`TIME`、`DATE`、`DATETIME`和`TIMESTAMP`

- 字符串类型，包括`CHAR`、`VARCHAR`、`BINARY`、`VARBINARY`、`BLOB`、`TEXT`、`ENUM`、`SET`

### 整数类型

数值型数据类型主要用来存储数字，MySQL 提供了多种数值数据类型，不同的数据类型提供不同的取值范围，
可以存储的值范围越大，其所需要的存储空间也会越大。

整数类型的属性字段可以添加 AUTO_INCREMENT 自增约束条件

- TINYINT，很小的整数，1 个字节
- SMALLINT，小的整数，2 个字节
- MEDIUMINT，中等大小的整数，3 个字节
- INT，普通大小的整数，4 个字节
- BIGINT，大整数，8 个字节

`INT(11)`后面的数字 11，表示该数据类型指定的显示宽度，指定能够显示的数值中数字的个数

> 显示宽度只用于显示，并不能限制取值范围和占用空间

### 浮点数类型和定点数类型

MySQL 中使用浮点数和定点数来表示小数，浮点类型有两种，单精度浮点类型(FLOAT，4 个字节)和双精度
浮点类型(DOUBLE，8 个字节)。定点类型只有一种 DECIMAL

> 浮点类型和定点类型都可以用(M,N)来表示，其中 M 称为精度表示总共的位数，N 称为标度表示小数的位数
> 不论定点和浮点类型，如果用户指定的精度超出精度范围，则会四舍五入进行处理

在 MySQL 中，定点数以字符串形式存储，在对精度要求比较高的时候(如货币、科学数据等)使用 DECIMAL 的
类型比较好，另外两个浮点数进行减法和比较运算时也容易出问题

### 日期与时间类型

MySQL 中有多种表示日期的数据类型

- YEAR，YYYY，1901 ～ 2155，1 个字节

- TIME，HH:MM:SS，-838:59:59 ～ 838:59:59，3 个字节

- DATE，YYYY-MM-DD，1000-01-01 ～ 9999-12-31，3 个字节

- DATETIME，YYYY-MM-DD HH:MM:SS，1000-01-01 00:00:00 ～ 9999-12-31 23:59:59，8 个字节

- TIMESTAMP，YYYY-MM-DD HH:MM:SS，1970-01-01 00:00:01 ～ 2038-01-19 03:14:07，4 个字节

### 字符串类型

字符串类型用来存储字符串数据，除了可以存储字符串数据之外，还可以存储其他数据，比如图片
和声音的二进制数据。字符串可以进行区分或者不区分大小写的串比较

- CHAR(M)，固定长度非二进制字符串，M 字节，1<= M <=255

- VARCHAR(M)，变长非二进制字符串，L+1 字节，L<=M 1<=M<=255

- TINYTEXT，非常小的非二进制字符串，L+1 字节，L<2^8

- TEXT，小的非二进制字符串，L+2 字节，L<2^16

- MEDIUMTEXT，中等大小的非二进制字符串，L+2 字节，L<=2^24

- LONGTEXT，大的非二进制字符串，L+4 字节，L<=2^32(大约是 4G)

- ENUM，枚举类型，只能有一个枚举字符串值，1 或 2 个字节，取决于枚举值的数目(最大值 65535)

- SET，一个设置，字符串对象可以有零个或多个 SET 成员，1，2，3，4 或 8 个字节，取决于成员的数量(最多 64 个成员)

VARCHAR、BLOB、TEXT 类型时变长类型，对于其存储需求取决于列值的实际长度(在上面介绍的用 L 表示)

一个 VARCHAR(10)列能保存最大长度为 10 个字符的一个字符串，实际的存储需要是字符串的长度 L，加上 1 个字节以记录
字符串的长度

### CHAR 和 VARCHAR

CHAR(M) 为固定长度字符串，在定义时指定字符串列长，当保存时右侧填充空格以达到指定的长度。M 表示列长度，
M 的范围是`0~255`个字符。
CHAR(4)定义了一个固定长度的字符串列，其包含的字符个数最大为 4，当检索到 CHAR 值时，尾部的空格将被删除掉

VARCHAR(M)是长度可变的字符串，M 表示最大列长度，M 的范围是`0~65535`，VARCHAR 的值保存和检索时尾部的空格
仍保留

### ENUM 类型

ENUM 是一个字符串对象，其值为表创建时在列规定中枚举的一列值

```
字段名 ENUM('值1','值2',...'值n')
```

ENUM 类型的字段在取值时，只能在指定的枚举列表中取，而且一次只能取一个。如果创建的成员中有空格时，其尾部的空格
将自动被删除。ENUM 值在内部用整数表示，每个枚举值均有一个索引值，列表值所允许的成员值从 1 开始编号，MySQL 存储
的就是这个索引编号。枚举最多可以有`65535`个元素

ENUM 列总有一个默认值，如果将 ENUM 列声明为 NULL，NULL 值则为该列的一个有效值，并且默认值为 NULL。
如果 ENUM 列被声明为 NOT NULL，其默认值为允许的值列表的第一个元素

### SET 类型

SET 是一个字符串对象，可以有零或多个值，SET 列最多可以有 64 个成员，其值为表创建时规定的一列值。
指定包含多个 SET 成员的 SET 列值时，各成员之间用逗号隔开

```
SET('值1','值2',...'值n')
```

与 ENUM 类型相同，SET 值在内部用整数表示，列表中每一个值都有一个索引编号。当创建表时，SET 成员值的
尾部空格将自动删除，但与 ENUM 类型不同的是，ENUM 类型的字段只能从定义的列值中选择一个值插入，而 SET 类型
的列可以从定义列值中选择多个字符的联合

如果插入 SET 字段中列值有重复，则 MySQL 自动删除重复的值，插入 SET 字段的值的顺序并不重要，MySQL 会在存入数据库
时，按照定义的顺序显示，如果插入了不正确的值，默认情况下 MySQL 将忽略这些值并给出警告

## 二进制类型

MySQL 支持两类字符型数据，文本字符串和二进制字符串。MySQL 中的二进制数据类型有 BIT、BINARY、
VARBINARY、TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB

- BIT，位字段类型，(M+7)/8 个字节

- BINARY(M)，固定长度二进制字符串，M 个字节

- VARBINARY(M)，可变长度二进制字符串，M+1 个字节

- TINYBLOB(M)，非常小的 BLOB，L+1 字节，L<=2^8

- BLOB(M)，小 BLOB，L+2 字节，L<=2^16

- MEDIUMBLOB(M)，中等大小的 BLOB，L+3 字节，L<=2^24

- LONGBLOB(M)，非常大的 BLOB，L+4 个字节，L<=2^32

### BIT 类型

位字段类型，M 表示每个值的位数，范围为`1~64`，如果 M 被省略，默认为 1。如果为 BIT(M)列分配的
值的长度小于 M 位，在值的左边用 0 填充。例如为 BIT(6)分配一个`101`，其效果与分配`000101`相同

MySQL 可以插入超出该列允许范围的值，但是需要对数据进行裁剪

### BLOB 类型

BLOB 是一个二进制大对象，用来存储可变数量的数据。

### 如果选择数据类型

MySQL 提供了大量的数据类型，为了优化存储，提高数据库的性能，在任何情况下均应使用最精确的类型。

BLOB 是二进制字符串，TEXT 是非二进制字符串，两者都可以存放大容量的信息，BLOB 主要存储图片、音频信息等，
而 TEXT 只能存储纯文本文件

## 常见运算符介绍

运算符连接表达式中各个操作数，其作用是用来指明对操作数所进行的运算，常见的运算有数学运算、比较运算、
位运算以及逻辑运算

### 算术运算符

- `+` 加法运算

- `-` 减法运算

- `*` 乘法运算

- `/` 除法运算，返回商

- `%` 求余运算，返回余数

在数学运算时，除数为 0 的除法是没有意义的，因此除法运算中的除数不能为 0，如果被 0 除，则返回
结果为 NULL

### 比较运算符

- `=` 等于

- `<=>` 安全等于

- `<>` 或 `!=` 不等于

- `<=` 小于等于

- `>=` 等于等于

- `>` 大于

- `IS NULL` 判断一个值是否为 NULL

- `IS NOT NULL` 判断一个值是否不为 NULL

- `LEAST` 在两个或多个参数时，返回最小值

- `GREATEST` 当有 2 个或多个参数时，返回最大值

- `BETWEEN AND` 判断一个值是否落在两个值之间

- `IN` 判断一个值是 IN 列表中的任意一个值

- `NOT IN` 判断一个值不是 IN 列表中的任意一个值

- `LIKE` 通配符匹配

- `REGEXP` 正则表达式匹配

### 逻辑运算符

- `NOT`或者`!` 逻辑非

- `AND`或者`&&` 逻辑与

- `OR`或者`||` 逻辑或

- `XOR` 逻辑异或

### 位运算符

位运算符是用来对二进制字节中的位进行测试、移位或者测试处理。

- `|` 位或

- `&` 位与

- `^` 位异或

- `<<` 位左移

- `>>` 位右移

- `~` 位取反，反转所有比特

### 运算符的优先级

最低: 赋值运算符`=`、`:=`

最高: !

不同的运算符优先级是不同的，一般情况下，级别高的运算符先进行计算，如果级别相同，MySQL 按表达式的
顺序从左到右依次计算

可以使用括号来改变优先级，这样会使计算过程更加清晰

### MySQL 中如何使用特殊字符

诸如单引号`'`双引号`"`反斜线`\`等符号，这些符号在 MySQL 中不能直接输入使用，否则会产生意料之外的结果。
在 MySQL 中，这些特殊字符称为转义字符，在输入时需要以反斜线符号`\`开头，所以使用单引号和双引号时应分别
输入`\'`和`\"`，反斜线时应该输入`\\`

### MySQL 中可以存储文件吗

MySQL 中的 BLOB 和 TEXT 字段类型可以存储数据量较大的文件，可以使用这些数据类型存储图像、声音或者大容量
的文本内容。例如网页或者文档，虽然使用 BLOB 或 TEXT 可以存储大容量的数据，但是这些字段的处理会降低数据库
的性能。如果并非必要，可以选择只存储文件的路径

### MySQL 中如何执行区分大小写的字符串比较

在 Windows 平台下，MySQL 是不区分大小写的，如果想执行区分大小写的比较，可以在字符串前面添加 BINARY

```
BINARY'a'='A'
// 0
```
