# 数据备份与还原

尽管采取了一些管理措施来保证数据库的安全，但是不确定的意外情况总是有可能造成数据的损失。保证数据安全
的最重要的一个措施是确保对数据进行定期备份。如果数据库中的数据丢失或出现错误，可以使用备份的数据进行
还原，这样就尽可能的降低了意外原因导致的损失

MySQL 提供了多种方法对数据进行备份和还原

## 数据备份

数据备份是数据库管理员非常重要的工作，系统意外崩溃或者硬件的损坏都可能导致数据库的丢失，
因此 MySQL 管理员应该定期地备份数据库，使得在意外发生时，尽可能减少损失

### 使用 mysqldump 命令备份

mysqldump 是 MySQL 提供的一个非常有用的数据库备份工具，mysqldump 命令执行时，可以将数据库
备份成一个文本文件，该文件中实际上包含了多个`CREATE` 和`INSERT`语句，使用这些语句可以重新创建表
和插入数据

```sql
mysqldump -u user -h host -ppassword dbname[tbname[,tbname...]] > filename.sql
```

示例

```
mysqldump -u root -p hrs > /home/myback/hrs_20220223.sql
```

备份文件中的`--`字符开头的行为注释语句，以`/*!`开头`*/`结尾的语句为可执行的 MySQL 注释，这些语句
可以被 MySQL 执行，但在其他数据库管理系统将被作为注释忽略，这可以提高数据库的可移植性

另外注意到，备份文件开始的一些语句以数字开头，这些数字代表了 MySQL 版本号，该数字告诉我们，这些语句
只有在指定的 MySQL 版本或者比该版本高的情况下才能执行。例如`40101`表明这些语句只有在 MySQL 版本号
`4.01.01`或者更高的条件下才可以被执行

### 使用 mysqldump 备份数据库中的某个表

```sql
mysqldump -u uesr -h host -p dbname [tbname[,tbname...]] > filename.sql
```

tbname 表示数据库中的表名，多个表名之间用空格隔开

示例

```sql
mysqldump -u root -p hrs tb_dept > /home/myback/hrs_tb-dept_20220223.sql
```

### 备份多个数据库

```
mysqldump -u user -h host -p --databases [dbname, [dbname...]] > filename.sql
```

使用`--databases`参数之后，必须指定至少一个数据库的名称，多个数据库名称之间用空格隔开

使用`--all-databases`参数可以备份系统中所有的数据库

```
mysqldump -u root -h host -p --all-databases > filename.sql
```

### 参数说明

- `--no-data` or `-d` 不写表的任何信息，只转储表的结构

## 数据还原

管理员操作失误，计算机故障以及其他意外情况，都会导致数据的丢失和破坏。当数据丢失或意外破坏时，
可以通过还原已经备份的数据尽量减少数据丢失和破坏造成的损失

### 使用 mysql 命令还原

备份的 sql 文件包含 CREATE、INSERT 语句，mysql 命令可以直接执行文件中的这些语句

```
mysql -u user -p [dbname] < filename.sql
```

dbname 是数据库名，如果 filename.sql 文件为 mysqldump 工具创建的包含创建数据库语句的文件，执行的时候
不需要指定数据库名

如果已经登录 MySQL 服务器，还可以使用 source 命令导入 sql 文件，source 语句的语法

```
source filename
```

使用 root 用户登录到服务器，然后使用 source 导入本地的备份文件

```sql
-- 选择要恢复的数据库
use booksDB

-- 使用source 命令导入备份文件
source /home/myback/booksDB_20110101.sql
```

> 执行 source 命令前，必须使用 use 语句选择数据库

## 直接复制数据库目录

这种方式只对 MyISAM 引擎的表有效，对于 InnoDB 引擎的表不可用

对于 Linux/Unix 操作系统来说，复制完文件需要将文件的用户和组更改为 mysql 运行的用户和组，
通常用户是 mysql，组也是 mysql

```
chown -R mysql.mysql /var/lib/mysql/dbname
```

> 如果需要恢复的数据库已经存在，则使用 DROP 语句删除已经存在的数据库之后，恢复才能成功

## 数据库迁移

数据库迁移就是把数据从一个系统移动到另一个系统上，数据迁移有以下原因

需要安装新的数据库服务器

MySQL 版本更新

数据库管理系统的变更，如从 Microsoft SQL Server 迁移到 MySQL

### 相同版本的 MySQL 数据库之间的迁移

相同版本的 MySQL 数据库之间的迁移就是主版本号相同的 MySQL 数据库之间进行数据库移动。
迁移过程其实就是在源数据库备份和目标数据库还原的组合

最常用和最安全的方式是使用 mysqldump 命令导出数据，然后在目标数据库服务器使用 mysql 命令导入

将www.abc.com主机上 MySQL 数据库全部迁移到www.bcd.com主机上，在www.abc.com 主机上执行的命令

```
mysqldump -h www.abc.com -uroot -ppassword dbname | mysql -h www.bcd.com -uroot -ppassword
```

mysqldump 导入的数据直接通过管道符`|`，传给 mysql 命令导入到主机www.bcd.com数据库中，dbname 为需要迁移
的数据库名称，如果要迁移全部的数据库，可使用参数`--all-databases`

### 不同版本的 MySQL 数据库之间的迁移

因为数据库升级等原因，需要将较旧版本 MySQL 数据库中的数据迁移到较新版本的数据库中。MySQL 服务器升级时，需要先停止服务，
然后卸载旧版本，并安装新版本的 MySQL，这种更新方法很简单，如果想保留旧版本中的用户访问控制信息，则需要备份 MySQL 中的 mysql
数据库，在新版本 MySQL 安装完成之后，重新读入 mysql 备份文件中的信息

旧版本与新版本的 MySQL，可能使用不同的默认字符集。如果数据库中有中文数据的，迁移过程中需要对默认字符集进行修改，不然可能显示无法正常

## 表的导出和导入

有时会需要将 MySQL 数据库中的数据导出到外部存储文件中，MySQL 数据库中的数据可以导出成 sql 文本文件、xml 文件或者 html 文件。

```sql
SELECT ... INTO OUTFILE 'filename'
```

可以把被选择的行写入一个文件中，filename 不能是一个已经存在的文件

该语句可以非常快速地把一个表转储到服务器上，如果想要在服务器主机之外的部分客户主机上创建
结果文件，不能使用`SELECT ...INTO OUTFILE`。这种情况下，应该在客户主机上使用
`mysql -e "SELECT..." > file_name`来生成文件

示例

```sql
SELECT * FROM test.person INTO OUTFILE "/home/myback/person.txt"
```

使用 FIELDS 选项和 LINES 选项，要求字段之间使用"," 间隔，所有字段值用双引号括起来，
定义转义字符为单引号`\'`，执行命令如下

```sql
SELECT * FROM hrs.tb_emp INTO OUTFILE "/Users/chengqunzhong/myback/file/tb_emp1.txt"
FIELDS
  TERMINATED BY ','
  ENCLOSED BY '\"'
  ESCAPED BY "\'"
LINES
  TERMINATED BY '\n';
```

## 用 mysql 命令导出文本文件

mysql 是一个功能丰富的工具命令，使用 mysql 还可以在命令行模式下执行 SQL 指令，将查询结果导入到
文本文件中

如果 MySQL 服务器是单独的机器，用户是在一个 client 上进行操作，用户要把数据结果导入到 client 机器上，
可以使用`mysql -e`

```
mysql -u root -p --execute="SELECT 语句" dbname > filename.txt
```

可以使用 `--vartical` 参数，将每条记录分为多行显示

```
mysql -u root -p --vertical --execute="SELECT * FROM tb_emp;" hrs > /Users/chengqunzhong/myback/file/tb_emp3.txt
```

mysql 可以将查询结果导出到 html 文件中，使用`--html`选项

```
mysql -u root -p --html --execute="SELECT * FROM tb_emp;" hrs > /Users/chengqunzhong/myback/file/tb_emp.html
```

## 使用 LOAD DATA INFILE 方式导入文本文件

MySQL 允许将数据导出到外部文件，也可以从外部文件导入数据。MySQL 提供了一些导入数据的工具，这些工具有
`LOAD DATA`语句、source 命令、mysql 命令

LOAD DATA INFILE 语句用于高速地从一个文件中读取行，并装入一个表中，文件名称必须为文字字符串

```sql
LOAD DATA INFILE 'filename.txt' INTO TABLE tablename [OPTIONS][IGNORE number LINES]
```

示例

```sql
LOAD DATA INFILE '/Users/chengqunzhong/myback/file/tb_emp.txt' INTO TABLE tb_emp;
```

> 有可能出现字符集问题

## 使用 mysqlimport 命令导入文本文件

```
mysqlimport -u root -p dbname file
```

## 如何选择备份工具

直接复制数据库文件是最为直接、快速的备份方法，但缺点是基本上不能实现增量备份。备份时必须确保
没有使用这些表，如果在复制一个表的同时服务器正在修改它，则复制无效。备份文件时，最好关闭服务器，
然后重新启动服务器，为了保证数据的一致性，需要在备份文件前，执行以下 SQL 语句

```
FLUSH TABLES WITH READ LOCK;
```

也就是把内存中的数据都刷到磁盘中，同时锁定数据表，以保证复制过程中不会有新的数据写入。这种方法备份出来的
数据恢复也很简单，直接复制回原来的数据库目录下即可

mysqldump 将数据表导成 SQL 脚本文件，在不同的 MySQL 版本之间升级时比较合适，这也是最常用的备份方法

## 使用 mysqldump 备份整个数据库成功，把表和数据库都删除了，但使用备份文件却不能恢复数据库

出现这种情况，是因为备份的时候没有指定`--databases`参数。默认情况下，如果只指定数据库名称，
mysqldump 备份的是数据库中所有的表，而不包括数据库的创建语句

示例

```
mysqldump -u root -p --databases bookdb > /home/myback/bookdb_20220225.sql
```
