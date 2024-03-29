# 数据库的基本操作

**本教程是基于 MacOS 示例**

### MySQL 服务的操作命令

查看服务状态

```
mysqladmin -u root -p status
```

命令行登录 MySQL

```bash
mysql -u root -p

# ******
```

MySQL 安装完成之后，将会在其 data 目录下自动创建几个必须的数据库，查看当前存在的数据库

```bash
# 大小写都可
SHOW DATABASES;
```

mysql 数据库是必需的，它描述用户访问权限。

创建数据库是在系统磁盘上划分一块区域用于数据的存储和管理

### 创建数据库

MySQL 中创建数据库的基本 SQL 语法格式为:

```
CREATE DATABASE database_name;
```

`database_name`为要创建的数据库的名称，该名称不能与已经存在的数据库重名

数据库创建好以后，可以使用`SHOW CREATE DATABASE` 声明查看数据库的定义:

```
SHOW CREATE DATABASE database_name\G;
```

### 删除数据库

删除数据库是将已经存在的数据库从磁盘空间上删除，清除之后，数据库中的所有数据也将一同被删除

```
DROP DATABASE database_name;
```

`database_name`为要删除的数据库名称，如果指定的数据库不存在，则删除出错

> 使用`DROP DATABASE` 命令时要非常谨慎，在执行该命令时，MySQL 不会给出任何提醒确认
> 信息，删除数据库后，数据库中存储的所有数据表和数据也将一同被删除，且不能恢复

## 数据库存储引擎

数据库存储引擎是数据库底层软件组件，数据库管理系统使用数据引擎进行创建、查询、更新和删除
数据操作。不同的存储引擎提供不同的存储机制、索引技巧、锁定水平等功能

使用不同的存储引擎，还可以获得特定的功能，现在许多不同的数据库管理系统都支持多种
不同的数据引擎

可以用`SHOW ENGINES\G;`查看系统所支持的引擎类型，MySQL5.5 支持的存储引擎有，InnoDB、
MyISAM、Memory、Merge、Archive、Federated、CSV、BLACKHOLE 等

### InnoDB 存储引擎

InnoDB 事务型数据库的首先引擎，支持事务安全表(ACID)，支持行锁定和外键。InnoDB 支持外键完整性约束，
在存储表中的数据时，每张表的存储都按主键顺序存放，如果没有显示在表定义时指定主键，InnoDB 会为每一行生成一个
6 字节的 ROWID，并以此为主键

### MyISAM 存储引擎

MyISAM 基于 ISAM 存储引擎，并对其进行扩展，它是 web、数据仓储和其他应用环境下最常使用的存储引擎之一，
MyISAM 拥有较高的插入、查询速度，但不支持事务

### MEMORY 存储引擎

MEMORY 存储引擎将表中的数据存储到内存中，为查询和引用其他表数据提供快速访问

### 存储引擎的选择

一般的指标

- 存储限制

- 支持事务

- 支持全文检索

- 支持数索引

- 支持哈希索引

- 支持数据缓存

- 支持外键
