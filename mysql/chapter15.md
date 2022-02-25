# MySQl 日志

MySQL 日志记录了 MySQL 数据库日常操作和错误信息。MySQL 有不同的类型日志文件，从日志当中可以
查询到 MySQL 数据库的运行情况、用户操作、错误信息等，可以为 MySQL 管理和优化提供必要的信息。
对于 MySQL 的管理工作而言，这些日志文件是不可缺少的

## 日志简介

MySQL 日志主要分为 4 类，使用这些日志文件，可以查看 MySQL 内部发生的事情

- 错误日志，记录 MySQL 服务的启动、运行或停止 MySQL 服务时出现的问题

- 查询日志，记录建立的客户端连接和执行的语句

- 二进制日志，记录所有更改数据的语句，可以用于数据复制

- 慢查询日志，记录所有执行时间超过 long_query_time 的所有查询或不使用索引的查询

启动日志功能会降低 MySQL 数据库的性能，在查询非常频繁的 MySQL 数据库系统中，如果开启了通用
查询日志和慢查询日志，MySQL 数据库会花费很多时间记录日志，同时，日志会占用大量的磁盘空间

## 二进制日志

二进制日志主要记录 MySQL 数据库的变化，二进制日志以一种有效的格式，并且是事务安全的方式包含更新
日志中可用的所有信息。二进制日志包含了所有更新了数据或者已经潜在更新了数据的语句。语句以事件的形式
保存，描述数据更新

二进制日志还包含关于每个更新数据库的语句的执行时间信息，它不包含没有修改任何数据的语句。如果要想
记录所有语句，需要使用一般查询日志，使用二进制日志的主要目的是最大可能地恢复数据库，因为二进制日志
包含备份后进行的所有更新

### 启动和设置二进制日志

默认情况下，二进制日志是关闭的，可以通过修改 MySQL 的配置文件来启动和设置二进制日志

```
log-bin [=path/filename]
expire_logs_days = 10
max_binlog_size = 100M
```

log-bin 定义开启二进制日志，path 表明日志文件所在目录路径，filename 指定了日志文件的名称，
如文件的全名 filename.000001 等

expire_logs_days 定义了 MySQL 清除过期日志的时间，即二进制日志自动删除的天数。默认值为 0，
表示没有自动删除

max_binlog_size 定义了单个文件的大小限制，如果二进制日志写入的内容大小超过给定值，日志就会发生滚动，
关闭当前文件，重新打开一个新的日志文件。不能将该变量设置为大于 1GB 或小于 4096 字节

添加完毕之后，关闭并重新启动 MySQL 服务进程，即可打开二进制日志，然后可以通过 SHOW VARIABLES 语句
来查询日志设置

```
show variables like 'log_%';
```

数据库文件最好不要与日志文件放在同一个磁盘上，这样，当数据库文件所在的磁盘发生故障时，可以使用
日志文件恢复数据

show binary logs 语句可以查看当前的二进制文件个数以其文件名

```
show binary logs;
```

日志文件的个数与 MySQL 服务启动的次数相同，每启动一次 MySQL 服务，将会产生一个新的日志文件

使用`mysqlbinlog`查看二进制日志文件

```
mysqlbinlog --no-defaults --set-charset=utf8 /usr/local/var/mysql/logs/mysql-bin.000001
```

> 查看 binlog 一般需要 root 权限
> sudo mysqlbinlog --no-defaults --set-charset=utf8 /usr/local/var/mysql/logs/mysql-bin.000001

## 删除二进制日志

MySQL 的二进制文件可以配置自动删除，同时 MySQL 也提供了安全的手动删除二进制文件的方法

```
RESET MASTER
```

执行完该语句，所有二进制日志将被删除

**PUREGE MASTER LOGS**删除指定日志文件

```
PURGE {MASTER|BINARY} LOGS TO 'log_name'
```

执行命令将删除文件名编号比指定的文件名编号小的所有日志文件

```
PURGE {MASTER|BINARY} LOGD BEFORE 'date'
```

删除指定日期以前的所有日志文件

示例

```
PURGE MASTER LOGS BEFORE '20110803';
```

### 使用二进制日志还原数据库

可以使用 mysqlbinlog 工具从指定的时间点开始直到现在

```
mysqlbinlog [option] filename | mysql -uuser -ppassword
```

`--start-position`(`--start-date`) 指定恢复数据的开始位置
`--stop-position`(`--stop-date`) 恢复数据结束位置

示例，使用 mysqlbinlog 恢复 MySQL 数据库到 2011 年 8 月 30 日 15:27:48 时的状态

```
mysqlbinlog --stop-date="2011-08-30 15:27:48" /usr/local/var/mysql/logs/binlog.000008 | mysql -uuser -ppass
```

根据`binlog.000008`日志文件恢复 2011-08-30 15:27:48 以前的所有操作

### 暂时停止二进制日志功能

**暂停记录二进制日志**

```
SET sql_log_bin = 0
```

**恢复记录二进制日志**

```
SET sql_log_bin = 1
```

## 错误日志

查看错误日志

```
SHOW VARIABLES LIKE 'log_error';
```

### 删除错误日志

MySQL 的错误日志是以文本文件的形式存在，可以直接删除

删除日志之后，如果需要重建日志文件，可以在服务端执行以下命令

```
mysqladmin -u root -p flush-logs
```

或在客户端登录 MySQL 数据库，执行

```
mysql> flush logs;
```

## 通用查询日志

通过查询日记记录 MySQL 的所有用户操作，包括启动和关闭服务、执行查询和更新语句等

### 启动和设置通用查询日志

```
[mysqld]
log[= path/[filename]]
```

## 通过查询日志

```
SET GLOBAL general_log = 'ON';
SET GLOBAL general_log = 'OFF';
```

```
[mysqld]
log[/usr/local/var/mysql/logs/mysql-general]
```
