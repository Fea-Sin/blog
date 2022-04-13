# 数据表的基本操作

在数据库中，数据表是数据库中最重要、最基本的操作对象，是数据存储的基本单位。数据表
被定义为列的集合，数据在表中是按照行和列的格式来存储的。每一行代表一条唯一的记录，每一列
代表记录中的一个域

## 创建数据表

所谓创建数据表，指的是在已经创建好的数据库中建立新表。创建数据表的过程是规定数据列的属性的过程，
同时也是实施数据完整性(包括实体完整性、引用完整性和域完整性)约束的过程

数据表属于数据库，在创建数据表之前，应该使用语句`USE <数据库名>`指定操作是在哪个数据库中进行

创建数据表的语法规则:

```
CREATE TABLE <表名>
(
  字段名1,数据类型[列级别约束条件][默认值],
  字段名2,数据类型[列级别约束条件][默认值],
  ....
  [表级别约束条件]
);
```

使用`CREATE TABLE`创建表时，必须指定以下信息:

- 要创建的表的名称，不区分大小写，不能使用 SQL 语言中的关键字，如`DROP`、`ALTER`、`INSERT`等

- 数据表中的每一个列(字段)的名称和数据类型，如果创建多个列，要用逗号分隔

使用`SHOW TABLES;`语句查看数据表

### 使用主键约束

主键分为两种类型，单字段主键和多字段联合主键

**单字段主键**

- 在定义列的同时指定主键

```
字段名 数据类型 PRIMARY KEY [默认值]
```

实例

```
CREATE TABLE tb_emp2
(
  id INT(11) PRIMARY KEY;
  name VARCHAR(25)
);
```

- 在定义所有列之后指定主键

```
[CONSTRAINT <约束名>] PRIMARY KEY[字段名]
```

实例

```
CREATE TABLE tb_emp3
(
  id INT(11),
  name VARCHAR(25),
  salary FLOAT,
  PRIMARY KEY(id)
);
```

**多字段联合主键**

```
PRIMARY KEY[字段1,字段2,...,字段n]
```

实例

```
CREATE TABLE tb_emp4
(
  name VARCHAR(25),
  deptId INT(11),
  salary FLOAT,
  PRIMARY KEY(name, deptId)
);
```

### 使用外键约束

外键用来在两个表的数据之间建立链接，它可以是一列或者多列，一个表可以有一个或多个外键。

一个表的外键可以为空，若不为空，则每一个外键值必须等于另一个表中主键的某个值

**外键**首先它是表中的一个字段，它可以不是本表的主键，但对应另外一个表的主键。外键主要
作用是保证数据引用的完整性，定义外键后，不允许删除在另一个表中具有关联关系的行。例如，部门
表 tb_dept 的主键是 id，在员工表 tb_emp 中有一个键 deptId 与这个 id 关联

**主表**对于两个具有关联关系的表而言，相关联字段中主键所在的那个表即是主表

**从表**对于两个具有关联关系的表而言，相关联字段中外键所在的那个表即是从表

创建外键的语法

```
[CONSTRAINT <外键名>] FOREIGN KEY 字段名1[字段名2,...] REFERENCES<主表名> 主键列1[主键列2,...]
```

`外键名`为定义的外键约束的名称，一个表中不能有相同名称的外键

### 使用非空约束

非空约束(Not Null Constraint)指字段的值不能为空，对于使用了非空约束的字段，如果用户在
添加数据时没有指定值，数据库系统会报错

```
字段名 数据类型 NOT NULL
```

### 使用唯一性约束

唯一性约束(Unique Constraint)要求该列唯一，允许为空，但只能出现一个空值，
唯一约束可以确保一列或几列不出现重复值

```
字段名 数据类型 UNIQUE
```

实例

```
CREATE TABLE tb_dept2
(
  id INT(11) PRIMARY KEY,
  name VARCHAR(25) UNIQUE,
  location VARCHAR(50)
);
```

**UNIQUE 与 PRIMARY 的区别**

一个表中可以有多个字段声明为 UNIQUE，但只能有一个 PRIMARY KEY 声明，PRIMARY KEY 的列
不允许有空值，但声明为 UNIQUE 的字段允许空值存在

### 使用默认约束

默认约束(Default Constraint)指定某列的默认值，如果插入一条新的记录时没有这个字段赋值，
系统会自动为这个字段赋值

```
字段名 数据类型 DEFAULT 默认值
```

实例

```
CREATE TABLE tb_emp3
(
  id INT(11) PRIMARY KEY,
  name VARCHAR(25) NOT NULL,
  deptId INT(11) DEFAULT 111,
  salary FLOAT
);
```

### 设置表的属性值自动增加

在数据库应用中，经常希望在每次插入新记录时，系统自动生成字段的主键值，可以通过为
表主键添加`AUTO_INCREMENT`关键字来实现。
默认的，MySQL 中`AUTO_INCREMENT`的初始值是 1，每新增一条记录，字段值自动加 1，一个表
只能有一个字段使用`AUTO_INCREMENT`约束，且该字段必须为主键的一部分

```
字段名 数据类型 AUTO_INCREMENT
```

## 查看数据表结构

查看表基本结构语句

```
DESCRIBE 表名;
```

或简写为

```
DESC 表名;
```

查看表详细结构语句

```
SHOW CREATE TABLE <表名>\G;
```

## 修改数据表

### 修改表名

```
ALTER TABLE <旧表名> RENAME <新表名>;
```

### 修改字段的数据类型

```
ALTER TABLE <表名> MODIFY <字段名> <数据类型>
```

### 修改字段名

```
ALTER TABLE <表名> CHANGE <旧字段名> <新字段名> <新数据类型>;
```

旧字段名指修改前的字段名，新字段名指修改后的字段名，新数据类型指修改后的
数据类型，如果不需要修改字段的数据类型，可以将新数据类型设置成与原来一样，但数据类型
不能为空

### 添加字段

随着业务需求的变化，可能需要在已经存在的表中添加新的字段，一个完整字段包括字段名、数据类型、
完整性约束

```
ALTET TABLE <表名> ADD <新字段名> <数据类型> [约束条件] [FIRST|AFTER 已存在字段名];
```

### 删除字段

```
ALTER TABLE <表名> DROP <字段名>;
```

### 修改字段的排列位置

对于一个数据表来说，在创建的时候，字段在表中的排列顺已经确定。但表的结构并不是完全不可以改变的

```
ALTER TABLE <表明> MODIFY <字段1> <数据类型> FIRST|AFTER <字段2>;
```

### 更改表的存储引擎

```
ALTER TABLE <表名> ENGINE=<更改后的存储引擎名>;
```

### 删除表的外键约束

对于数据库中定义的外键，如果不再需要，可以将其删除，外键一旦删除，就会删除主表和从表的
关联关系

```
ALTER TABLE <表名> DROP FOREIGN KEY <外键约束名>;
```

### 删除数据表

删除数据表就是将数据库中已经存在的表从数据库中删除，在删除表的同时，表的定义和表中所有的数据
均会被删除，因此，在进行删除操作前，最好对表中的数据做个备份

**删除没有被关联的表**

```
DROP TABLE [IF EXISTS] 表1,表2,...表n;
```

**删除被其他表关联的主表**

数据表之间存在外键关联的情况下，如果直接删除父表，结果会显示失败。原因是直接删除将破坏表的
参照完整性。如果必须删除，可以先删除与它关联的子表，再删除父表。

如果要单独删除父表，只需将关联的表的外键约束条件取消，然后就可以删除父表