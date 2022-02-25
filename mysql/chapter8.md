# 插入、更新与删除数据

存储在系统中的数据是数据库管理系统的核心，数据库被设计用来管理数据的存储、访问和维护数据
的完整性。MySQL 中提供了功能丰富的数据库管理语句，包括有效向数据库中插入数据的 INSERT 语句，
更新数据的 UPDATE 语句以及当数据不再使用时删除数据的 DELETE 语句

## 插入数据

MySQL 中使用 INSERT 语句向数据库中插入新的数据记录，可以插入的方式有，插入完整记录、插入多条
记录、插入另一个查询的结果

**为表的所有字段插入数据**

使用基本的 INSERT 语句插入数据要求指定表名和插入到新记录中的值

```sql
INSERT INTO table_name(column_list) VALUES(value_list);
```

向表中所有字段插入值的方法有两种，一种是指定所有字段名，另一种是完全不指定字段名

INSERT 语句后面的列名称顺序可以不是 person 表定义时的顺序，即插入数据时，不需要按照表定义的
顺序插入，只要保证值的顺序与列字段的顺序相同就可以

使用 INSERT 插入数据时，允许列名称列表为空，此时，值列表中需要为表的每一个字段指定值，并且值
的顺序必须和数据表中字段定义时的顺序相同

**为表的指定字段插入数据**

为表的指定字段插入数据，就是在 INSERT 语句中只向部分字段中插入值，而其他字段的值为表定义时的默认值

> 要保证每个插入值的类型和对应列的数据类型匹配，如果类型不同，将无法插入，并且 MySQL 会产生错误

**同时插入多条记录**

INSERT 语句可以同时向数据表中插入多条记录，插入时指定多个值列表，每个值列表之间用逗号分隔开

```sql
INSERT INTO table_name(column_list)
VALUES(value_list1), (value_list2), ..., (value_listn);
```

**将查询结果插入到表中**

INSERT 还可以将 SELECT 语句查询的结果插入到表中，如果想要从另外一个表中合并个人信息到 person 表，不需要
把每一条记录的值一个一个输入，只需要使用一条 INSERT 语句和一条 SELECT 语句组成的组合语句，即可快速地从一个
或多个表中向一个表中插入多个行

```sql
INSERT INTO table_name1(column_list1)
SELECT (column_list2) FROM table_name2 WHERE (condition);
```

## 更新数据

MySQL 中使用 UPDATE 语句更新表中的记录，可以更新特定的行或者同时更新所有的行

```sql
UPDATE table_name
SET column_name1 = value1, column_name2 = value2, ..., column_namen = valuen
WHERE (condition);
```

## 删除数据

从数据表中删除数据使用 DELETE 语句，DELETE 语句允许 WHERE 子句指定删除条件

```sql
DELETE FROM table_name [WHERE (condition)];
```

**更新或删除表时必须指定 WHERE 子句吗**

如果省略 WHERE 子句，则 UPDATE 和 DELETE 将被应用到表中所有的行，因此，除非确实打算更新或者删除
所有记录，否则要注意使用 WHERE 子句。
建议对表进行更新和删除之前，使用 SELECT 语句确认需要操作的数据，以免造成无法挽回的结果
