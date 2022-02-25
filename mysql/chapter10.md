# 存储过程和函数

简单的说，存储过程就是一条或者多条 SQL 语句的集合，可视为批文件，但是其作用不仅限于批处理。本章主要介绍如何创建
存储过程和存储函数以及变量的使用

## 创建存储过程和函数

存储程序可以分为存储过程和函数，MySQL 中创建存储过程和函数使用的语句分别是`CREATE PROCEDURE`和`CREATE FUNCTION`，
使用 CALL 语句来调用存储过程，只能用输出变量返回值。函数可以从语句外调用，通过引用函数名，也能返回标量值。存储过程
也可以调用其他存储过程

基本语法格式

```sql
CREATE PROCEDURE sp_name([proc_parameter]) [characteristics] routine_body
```

`sp_name` 为存储过程的名称，proc_parameter 为指定存储过程的参数列表

```
[IN|OUT|INOUT] param_name type
```

`param_name` 表示参数名称，`type`表示参数的类型

`routine_body` 是 SQL 代码的内容，可以用 BEGIN...END 来表示 SQL 代码的开始和结束

编写存储过程并不是件简单的事情，可能存储过程中需要复杂的 SQL 语句，并且要有创建存储过程的权限，
但是使用存储过程将简化操作，减少冗余的操作步骤，同时还可以减少操作过程的失误，提高效率，因此存储
过程是非常有用的，而且应该近可能的学会使用

> `DELIMITER //` 语句将 MySQL 的结束符设置为`//`，后续再使用`DELIMITER ;`将结束符恢复

### 创建存储函数

```sql
CREATE FUNCTION func_name([func_parameter])
  RETURNS type
[characteristic] routine_body
```

`func_name` 表示存储函数的名称

`RETURNS type` 语句表示函数返回数据的类型

> 指定参数为 IN、OUT、INOUT 只对 PROCEDURE 是合法的，FUNCTION 中总是默认为 IN 参数。
> RETURNS 子句只能对 FUNCTION 做指定，对函数而言这是强制的，它用来指定函数的返回类型，
> 而且函数体必须包含一个 RETURN value 语句

## 变量的使用

变量可以在子程序中声明并使用，这些变量的作用范围是在`BEGIN...END`程序中

**定义变量**

```sql
DECLARE var_name[, varname]... data_type [DEFAULT value];
```

定义名称为 myparam 的变量，类型为 INT，默认值为 100

```sql
DECLARE myparam INT DEFAULT 100;
```

**为变量赋值**

```sql
SET var_name = expr [, var_name = expr]...;
```

示例

```sql
DECLARE var1, var2, var3 INT;
SET var1 = 10, var2 = 20;
SET var3 = var1  + var2;
```

MySQL 中还可以通过`SELECT ... INTO`为一个或多个变量赋值

```sql
SELECT col_name[,...] INTO var_name[,...] table_expr;
```

示例

```sql
DECLARE fruitname CHAR(50);
DECLARE fruitprice DECIMAL(8,2);

SELECT f_name, f_price INTO fruitname, fruitprice
FROM fruits WHERE f_id = 'a1';
```

### 定义条件和处理程序

特定条件需要特定处理，这些条件可以联系到错误，以及子程序中的一般流程控制，定义条件是事件先定义程序
执行过程中遇到的问题，处理程序定义了在遇到这些问题时应采取的处理方式，并且保证存储过程或函数在遇到
警告或错误时能继续执行名，这样可以增强存储程序处理问题的能力，避免程序异常停止运行

**定义条件**

```sql
DECLARE condition_name CONDITION FOR [condition_type]

[condition_type]:
  SQLSTATE[VALUE] sqlstate_value | mysql_error_code
```

定义`ERROR 1148(42000)`错误，名称为`command_not_allowed`

方法一

```sql
DECLARE command_not_allowed CONDITION FOR SQLSTATE '42000';
```

方法二

```sql
DECLARE command_not_allowed CONDITION FOR 1148
```

### 定义处理程序

```sql
DECLARE handler_type HANDLER FOR condition_value[,...] sp_statement
handler_type:
  CONTINUE | EXIT | UNDO

condition_value:
   SQLSTATE [VALUE] sqlstate_value
   | condition_name
   | SQLWARNING
   | NOT FOUND
   | SQLEXCEPTION
   | mysql_error_code
```

`handler_type` 为错误处理方式

- CONTINUE 表示遇到错误不处理，继续执行
- EXIT 遇到错误马上退出
- UNDO 表示遇到错误后撤回之前的操作，MySQL 中暂时不支持这样的操作

`codition_value` 表示错误类型

`sp_statement` 参数为程序语句段，表示在遇到定义的错误时，需要执行的存储过程或程序

> `@var_name` 表示用户变量，使用 SET 语句为其赋值，用户变量与连接有关，一个客户端定义的变量
> 不能被其他客户端看到或使用。当客户端退出时，该客户端连接的所有变量将自动释放

## 光变的使用

查询语句可能返回多条记录，如果数据量非常大，需要在存储过程和存储函数中使用光标来逐条读取查询
结果集中的记录，应用程序可以根据需要滚动或浏览其中的数据

**声明光标**

```sql
DECLARE cursor_name CURSOR FOR select_statement
```

cursor_name 参数表示光标的名称，select_statement 参数表示 SELECT 语句的内容，返回一个
用于创建光标的结果集

```sql
DECLARE cursor_fruit CURSOR FOR SELECT f_name, f_price FROM fruits;
```

## 流程控制的使用

流程控制语句用来根据条件控制语句的执行，MySQL 中的用来构造控制流程的语句有，IF 语句、
CASE 语句、LOOP 语句、WHILE 语句、LEAVE 语句、ITERATE 语句、REPEAT 语句和 WHILE 语句

每个流程中肯能包含一个单独语句，或者是使用`BEGIN...END`构造的复合语句，构造可以被嵌套

**IF 语句**

```sql
IF expr_condition THEN statement_list
  [ELSEIF expr_condition THEN statement_list]...
  [ELSE statement_list]
END IF
```

> MySQL 中还有一个 IF()函数，它不同于这里描述的 IF 语句

```sql
IF val IS NULL
  THEN SELECT 'val is NULL';
  ELSE SELECT 'val is not NULL';
END IF;
```

**CASE 语句**
CASE 是另一个进行条件判断的语句

```sql
CASE case_expr
  WHEN when_value THEN statement_list
  [WHEN when_value THEN statement_list]...
  [ELSE statement_list]
END CASE
```

示例

```sql
CASE val
   WHEN 1 THEN SELECT 'val is 1';
   WHEN 2 THEN SELECT 'val is 2';
   ELSE SELECT 'val is not 1 or 2';
END CASE;
```

**LOOP 语句**

LOOP 循环语句用来重复执行某些语句，与 IF 和 CASE 语句相比，LOOP 只是创建一个循环操作的过程，
并不进行条件判断，LOOP 内的语句一直重复执行直到循环被退出，跳出循环过程使用 LEAVE 子句

```sql
[loop_label:]LOOP
  statement_list
END LOOP[loop_label]
```

示例

```sql
DECLARE id INT DEFAULT 0;

add_loop: LOOP
  SET id = id + 1;
  IF id >= 10 THEN LEAVE add_loop;
  END IF;
END LOOP add_loop;
```

`LEAVE`语句用来退出任何被标注的流程控制构造

```sql
LEAVE label
```

**ITERATE 语句**

ITERATE 语句将执行顺序转到语句段开头处

```sql
ITERATE label
```

ITERATE 只可以出现在 LOOP、REPEATE 和 WHILE 语句内，ITERATE 的意思为`再次循环`，
ITERATE 语句必须跟在循环标志前面

**REPEAT 语句**

REPEAT 语句创建一个带条件判断的循环过程，每次语句执行完毕之后，会对条件表达式进行判断，
如果表达式为真，则循环结束，否则重复执行循环中的语句

```sql
[repeatlabel:] REPEAT
  statement_list
UNTIL expr_condition
END REPEAT [repeat_label]
```

示例

```sql
DECLARE id INT DEFAULT 0;

REPEAT
  SET id = id + 1;
  UNTIL id >= 10
END REPEAT;
```

**WHILE 语句**

WHILE 语句创建一个带条件判断的循环过程，与 REPEAT 不同，WHILE 在执行语句执行时，
先对指定的表示进行判断，如果为真，则执行循环内的语句，否则退出循环

```sql
[while_label:] WHILE expr_condition DO
  statement_list
END WHILE [while_label]
```

示例

```sql
DECLARE i INT DEFAULT 0;
WHILE i < 10 DO
  SET i = i + 1;
END WHILE;
```

## 调用存储过程和函数

存储过程必须使用`CALL`语句调用，并且存储过程和数据库有关，如果要执行其他数据库中的存储过程，
需要指定数据库名称`CALL dbname.procname`

存储函数的调用与 MySQL 中预定义的函数调用方式相同

**调用存储过程**

```sql
CALL sp_name([parameter[,...]])
```

**调用存储函数**

用户自己定义的存储函数与 MySQL 内部函数是一个性质的，区别是，存储函数是用户自己定义的，而内部
函数是 MySQL 的开发者定义的

## 查看存储过程和函数

MySQL 存储了存储过程和函数的状态信息，用户可以使用 SHOW STATUS 语句或 SHOW CREATE 语句来查看，
也可以直接从系统的`information_schema`数据库中查询

查看存储过程和函数的状态

```sql
SHOW {PROCEDURE | FUNCTION} STATUS [LIKE 'pattern']
```

LIKE 语句表示匹配存储过程和函数的名称

```sql
SHOW PROCEDURE STATUS LIKE 'C%'\G;
```

**SHOW CREATE 语句查看存储过程和函数的定义**

```sql
SHOW CREATE {PROCEDURE | FUNCTION} sp_name;
```

示例

```sql
SHOW CREATE FUNCTION zoo.CountProc \G;
```

### 从 information_schema.Routines 表中查看存储过程和函数的信息

MySQL 中存储过程和函数的信息存储在`information_schema`数据库下的 Routines 表中

```sql
SELECT * FROM information_schema.Routines WHERE ROUTINE_NAME = 'sp_name';
```

## 修改存储过程和函数

使用 ALTER 语句可以修改存储过程或函数

```sql
ALTER {PROCEDURE | FUNCTION} sp_name[characteristic...]
```

## 删除存储过程和函数

```sql
DROP {PROCEDURE | FUNCTION} [IF EXISTS] sp_name;
```

这个语句被用来移除一个存储过程或函数，sp_name 为要移除的存储过程或函数的名称

`IF EXISTS` 子句是一个 MySQL 的扩展，如果程序或函数不存在，它可以防止发生错误，产生一个
用`SHOW WARNING` 查看的警告
