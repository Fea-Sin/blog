# 视图

数据库中的视图是一个虚拟表，同真实的表一样，视图包含一系列带有名称的行和列的数据，行和列数据来
自由定义视图查询所引用的表，并且在引用视图时动态生成

视图是从一个或多个表中导出的，视图的行为与表非常相似，但视图是一个虚拟表，在视图中用户可以使用 SELECT
语句查询数据，以及使用 INSET、UPDATE 和 DELETE 修改记录。从 MySQL 开始可以使用视图，视图可以使
用户操作方便，而且可以保障数据库系统的安全

视图还可以从已经存在的视图的基础上定义，视图一经定义便存储在数据库中，与其对应的数据并没有像表那样在数据
库中再存储一份，通过视图看到的数据只是存放在基本表中的数据。对视图的操作与对表的操作一样，可以对其进行
查询、修改和删除。当对通过视图看到的数据进行修改时，相应的基本表的数据也要发生变化，同时若基本表的数据
发生变化，则这种变化也可以自动反映到视图中

### 视图的作用

**简单化**
视图不仅可以简化对数据的理解，也可以简化他们的操作，那些被经常使用的查询可以被定义为视图，从而使得用户
不必为以后的操作每次指定全部的条件

**安全性**

通过视图用户只能查询和修改他们所能见到的数据，数据库中的其他数据则既看不见也取不到。数据库授权命令
可以使每个用户对数据库的检索限制到特定的数据库对象上，但不能授权到数据库特定行和特定的列上，通过
视图用户可以被限制在数据的不同子集上

**逻辑数据独立性**

视图可帮助用户屏蔽真实表结构变化带来的影响

## 创建视图

创建视图使用`CREATE VIEW`语句

```sql
CREATE [OR REPLACE][ALGORITHM = {UNDEFINED|MERGE|TEMPTABLE}]
  VIEW view_name [(column_list)]
  AS SELECT_statement
  [WITH[CASCADED|LOCAL]CHECK OPTION]
```

CREATE 表示创建新的视图，REPLACE 表示替换已经创建的视图，ALGORITHM 表示视图
选择的算法，view_name 为视图的名称，column_list 为属性列

视图属于数据库，在默认情况下，将在当前数据库创建视图。要想在给定数据库中明确创建视图，
应将名称指定为`db_name.view_name`

### 在单表上创建视图

示例

```sql
CREATE VIEW view_t AS SELECT quantity,price,quantity*price FROM t;
```

查询视图

```sql
SELECT * FROM view_t;
```

默认情况下创建的视图和基本表的字段是一样的，也可以通过指定视图字段的名称来创建视图

```sql
CREATE VIEW view_t2(qty, price,total) AS SELECT quantity, price, quantity*price FROM t;
```

### 在多表上创建视图

示例

```sql
CREATE VIEW stu_glass(id, name, glass) AS SELECT student.s_id, student.name, stu_info.glass
FROM student, stu_info WHERE student.s_id = stu_info.s_id;
```

## 查看视图

查看视图是查看数据库中已经存在的视图的定义，查看视图必须要有 SHOW VIEW 的权限，MySQL 数据库
下的 user 表中保存着这个信息

**DESCRIBE 语句查看视图基本信息**

```sql
DESCRIBE 视图名;
```

DESCRIBE 一般情况下都简写成`DESC`

**SHOW TABLE STATUS**语句查看视图基本信息

```sql
SHOW TABLE STATUS LIKE '视图名';
```

示例

```sql
SHOW TABLE STATUS LIKE 'stu_glass'\G;
```

**SHOW CREATE VIEW**语句查看视图详细信息

```sql
SHOW CREATE VIEW 视图名;
```

```sql
SHOW CREATE VIEW stu_glass\G;
```

## 修改视图

修改视图是指修改数据库中存在的视图，当基本表的某些字段发生变化的时候，可以通过修改视图来
保持与基本表的一致性。MySQL 中通过 CREATE OR REPLACE VIEW 语句和 ALTER 语句来修改视图

修改视图的语句和创建视图的语句是完全一样的，当视图已经存在的，修改语句对视图进行修改，当视图
不存在时，创建视图

```sql
CREATE OR REPLACE VIEW view_t AS SELECT * FROM t;
```

**ALTER**语句修改视图

```sql
ALTER [ALGORITHM = {UNDEFINED|MERGE|TEMPTABLE}]
  VIEW view_name[(column_list)]
  AS SELECT_statement
  [WITH[CASCADED|LOCAL]CHECK OPTION]
```

示例

```sql
ALTER VIEW view_t AS SELECT quantity FROM t;
```

## 更新视图

更新视图是指通过视图来插入、更新、删除表中的数据，因为视图是一个虚表，其中没有数据。
通过视图更新的时候都是转到基本表上进行更新的，如果对视图增加或者删除记录，实际上是对
其基本表增加或者删除记录

视图更新的 3 种方法，`INSERT`、`UPDATE`、`DELETE`

```sql
UPDATE view_t SET quantity=5;
```

```sql
DELETE FROM view_t2 WHERE price=5;
```

在视图 view_t2 中删除`price=5`的记录，视图中的删除操作最终是通过删除基本表中相关的记录实现的

## 删除视图

当视图不再需要时，可以将其删除，删除一个或多个视图可以使用`DROP VIEW`语句

```sql
DROP VIEW [IF EXISTS]
  view_name[, view_name,...]
  [RESTRICT|CASCADE]
```

示例

```sql
DROP VIEW IF EXISTS stu_glass;
```

**MySQL 中视图和表的区别以及联系时是什么？**

两者的区别

- 视图是已经编译好的 SQL 语句，是基于 SQL 语句的结果集的可视化的表，而表不是
- 视图没有实际的物理记录，而基本表有
- 表是内容，视图是窗口
- 表占用物理空间而视图不占用物理空间，视图只是逻辑概念的存在，表可以及时对它进行修改，但视图只能用创建
  的语句来修改
- 视图是查看数据表的一种方法，可以查询数据表中某些字段构成的数据，只是一些 SQL 语句的集合。从安全的角度
  来说，视图可以防止用户接触数据表，因而用户不知道表结构
- 表属于全局模式中的表，是实表。视图属于局部模式的表，是虚表
- 视图的建立和删除只影响视图本身，不影响对应的基本表

两者的联系

视图是在基本表之上建立的表，它的结构（所定义的列）和内容（即所有记录）都来自基本表，
它依据基本表存在而存在。
一个视图可以对应一个基本表，也可以对应多个基本表，视图是基本表的抽象和在逻辑意义上建立的新关系
