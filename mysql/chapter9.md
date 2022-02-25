# 索引

索引用于快速找出某个列中有一特定值的行。不使用索引，MySQL 必须从第 1 条记录开始读完整个表，直到找出
相关的行，表越大，查询数据所花费的时间越多。

### 索引简介

索引时对数据库表中一列或多列的值进行排序的一种结构，使用索引可提高数据库中特定数据的查询速度

索引是一个单独的、存储在磁盘上的数据结构，它们包含着对数据表里所有记录的引用指针。使用索引用于
快速找出在某个或多个列中有一特定值的行，所有 MySQL 列类型都可以被索引，对相关列使用索引是
提高查询操作速度的最佳途径

索引是在存储引擎中实现的，因此每种存储引擎的索引都不一定完全相同，并且每种存储引擎也不一定支持
所有索引类型。根据存储引擎定义每个表的最大索引数和最大索引长度

**索引的优点**

- 通过创建唯一索引，可以保证数据库表中每一行数据的唯一性

- 可以大大加快数据的查询速度，这是创建索引的最主要的原因

- 在实现数据的参考完整性方面，可以加速表和表之间的连接

- 在使用分组和排序子句进行数据查询时，也可以显著减少查询中分组和排序的时间

**索引的不利方面**

- 创建索引和维护索引要耗费时间，并且随着数据量的增加所耗费的时间也会增加

- 索引需要占磁盘空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果有大量的索引，
  索引文件可能比数据文件更快达到最大文件尺寸

- 当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，这样就降低了数据的维护速度

## 创建索引

MySQL 支持多种方法在单个或多个列上创建索引，在创建表的定义语句 CREATE TABLE 中指定索引列，
使用 ALTER TABLE 语句在存在的表上创建索引或者使用 CREATE INDEX 语句在已存在的表上添加索引

使用 CREATE TABLE 创建表时，除了可以定义列的数据类型，还可以定义主键约束、外键约束或者唯一性约束，
而不论创建那种约束，在定义约束的同时相当于在指定列上创建了一个索引

创建表时创建索引的基本语法格式

```sql
CREATE TABLE table_name[col_name data_type]
[UNIQUE|FULLTEXT|SPATIAL][INDEX|KEY][index_name](col_name[length])[ASC|DESC]
```

UNIQUE、FULLTEXT、SPATIAL 为可选参数，分别表示唯一索引、全文索引和空间索引

INDEX 与 KEY 为同义词，两者作用相同，用来指定创建索引，col_name 为需要创建索引的字段列，
该列必须从数据表中该定义的多个列中选择
index_name 指定索引的名称，为可选参数，如果不指定，MySQL 默认 col_name 为索引值，length 为可选参数，
表示索引的长度，只有字符串类型才能指定索引长度，ASC 或 DESC 指定升序或者降序的索引值存储

### 创建唯一索引

```sql
CREATE TABLE t1
(
  id INT NOT NULL,
  name CHAR(30) NOT NULL,
  UNIQUE INDEX UniqIdx(id)
);
```

### 创建单列索引

```sql
CREATE TABLE t2
(
  id INT NOT NULL,
  name CHAR(50) NULL,
  INDEX SingleIdx(name(20))
);
```

### 创建组合索引

```sql
CREATE TABLE t3
(
  id INT NOT NULL,
  name CHAR(30) NOT NULL,
  age INT NOT NULL,
  info VARCHAR(255),
  INDEX MultiIdx(id, name, age(100))
);
```

### 创建全文索引

FULLTEXT 全文索引可以用于全文搜索，只有 MyISAM 存储引擎支持 FULLTEXT 索引，并且
只为 CHAR、VARCHAR、TEXT 列，索引总是对整个列进行，不支持局部(前缀)索引

```sql
CREATE TABLE t4
(
  id INT NOT NULL,
  name CHAR(30) NOT NULL,
  age INT NOT NULL,
  info VARCHAR(255),
  FULLTEXT INDEX FullTextIdx(info)
)ENGINE=MyISAM;
```

## 在已经存在的表上创建索引

ALTER TABLE 创建索引的基本语法

```sql
ALTER TABLE table_name ADD [UNIQUE|FULLTEXT|SPATIAL][INDEX|KEY][index_name](col_name(length))[ASC|DESC]
```

使用`SHOW INDEX` 语句查看表中的索引

```sql
SHOW INDEX FROM book \G;
```

### 使用 CREATE INDEX 创建索引

```sql
CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX index_name
ON table_name(col_name[length])[ASC|DESC]
```

```sql
CREATE UNIQUE INDEX UniqidIdx ON book(bookId);
```

## 删除索引

MySQL 中删除索引使用 ALTER TABLE 或者 DROP INDEX 语句

```sql
ALTER TABLE table_name DROP INDEX index_name;
```

示例

```sql
ALTER TABLE book DROP INDEX UniqidIdx;
```

使用 DROP INDEX 语句删除索引

```sql
DROP INDEX index_name ON table_name;
```

示例

```sql
DROP INDEX BkAuAndInfoIdx ON book;
```

**索引对数据库性能如此重要，应该如何使用它**

为数据库选择正确的索引是一项复杂的任务，如果索引列较少，则需要的磁盘空间和维护开销都较少。
如果在一个大表上创建了多种组合索引，索引文件也会膨胀很快。
另一方面，索引较多可覆盖更多的查询，可能需要实验若干不同的设计，才能找到最有效的索引。
可添加、修改和删除索引而不影响数据库架构或应用程序设计，因此，应该尝试不同的索引从而建立最优的索引
