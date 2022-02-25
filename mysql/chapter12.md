# MySQL 函数

MySQL 的触发器和存储过程一样，都是嵌入到 MySQL 的一段程序。触发器是由事件来触发某个操作，
这些事件包括`INSERT`、`UPDATE`和`DELETE`语句
如果定义了触发程序，当数据库执行这些语句的时候就会激发触发器执行相应的操作，触发程序是与表有关
的命名数据库对象，当表上出现特定事件时，将激活该对象

## 创建触发器

触发器是个特殊的存储过程，不同的是，执行存储过程要使用`CALL`语句来调用，而触发器的执行不需要使用
CALL 语句来调用，也不需要手工启动，只要当一个预定义的事件发生的时候，就会被 MySQL 自动调用

触发器可以查询其他表，而且可以包含复杂的 SQL 语句，它们主要用于满足复杂的业务规则或要求

**创建只有一个执行语句的触发器**

```sql
CREATE TRIGGER trigger_name trigger_time trigger_event
ON tb1_name FOR EACH ROW trigger_stmt
```

`trigger_name` 触发器名称，用户自行指定
`trigger_time` 标识触发时机，可以指定为 before 或 after
`trigger_event` 标识触发事件，包括 INSERT、UPDATE、DELETE
`tb1_name` 标识建立触发器的表名，即在那张表上建立触发器
`trigger_stmt` 是触发器程序体，触发器程序可以使用 begin 和 end 作为开始和结束

示例

```sql
CREATE TRIGGER ins_sum BEFORE INSERT ON account
FOR EACH ROW SET @sum = @sum + NEW.amount;
```

创建一个名为 ins_sum 的触发器，触发的条件是向数据表 account 插入数据之前，对新插入的的
amount 字段进行求和计算

**创建有多个执行语句的触发器**

```sql
DELIMITER //

CREATE TRIGGER testref BEFORE INSERT ON test1
  FOR EACH ROW BEGIN
  INSERT INTO test2 SET a2 = NEW.a1;
  DELETE FROM test3 WHERE a3 = NEW.a1;
  UPDATE test4 SET b4 = b4 + 1 WHERE a4 = NEW.a1;

DELIMITER ;
```

在向 test1 插入记录的时候，test2、test3、test4 都发生了变化。INSERT 触发了触发器，
向 test2 表中插入了 test1 中的值，删除了 test3 中相同的内容，同时更新的了 test4 中的 b4

## 查看触发器

查看触发器是指查看数据库中已经存在的触发器的定义、状态、语法信息等

通过 SHOW TRIGGERS 查看触发器的语句

```sql
SHOW TRIGGERS;
```

or

```sql
SHOW TRIGGERS\G;
```

创建一个简单的触发器，名称为`trig_update`，每次向`account`表更新数据之后都会向名称为
myevent 的数据表中插入一条记录

```sql
CREATE TRIGGER trig_update AFTER UPDATE ON account
FOR EACH ROW INSERT INTO myevent VALUES(1, 'after update')
```

**直接从 information_schema**数据库中的 triggers 表查找触发器

```sql
SELECT * FROM INFORMATION_SCHEMA.TRIGGERS \G;
```

## 删除触发器

```sql
DROP TRIGGER [schema_name.]trigger_name
```

schema_name 表示数据库名称，是可选的，如果省略了 schema_name，将从当前数据库中删除触发器

示例

```sql
DROP TRIGGER trig_update;
```

### 使用触发器时须特别注意

在使用触发器的时候需要注意，对于相同的表，相同的事件只能创建一个触发器，比如对表 account 创建了一个
BEFORE INSERT 触发器，那么如果对表 account 再创建一个 BEFORE INSERT 触发器 MySQL 将会报错

### 及时删除不再需要的触发器

触发器定义之后，每次执行触发事件，都会激活触发器并执行触发器中的语句。如果需求发生
变化，而触发器没有进行相应的改变或者删除，则触发器仍然会执行旧的语句，从而会影响新的数据的
完整性

将不再使用的触发器及时删除
