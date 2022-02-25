## 问题一

`ERROR 1045 (28000): Access denied for user 'testUser'@'localhost' (using password: YES)`

用户名密码错误

## 问题二

有外键的表中数据无法删除

```sql
SET FOREIGN_KEY_CHECKS=0;
DELETE FROM `jobs` WHERE `job_id` = 1;
SET FOREIGN_KEY_CHECKS=1;
```

## 问题三

MySQL 导入导出文件提示 `--secure-file-priv option`问题

其原因是，安装 MySQL 的时候限制了导入和导出的目录权限，只允许规定的目录才能导入导出

查看目录

```sql
SHOW VARIABLES LIKE "secure_file_priv";
```

+------------------+-------+
| Variable_name | Value |
+------------------+-------+
| secure_file_priv | NULL |
+------------------+-------+

## 问题四

如何查看 MySQL 配置文件

```
mysql --verbose --help | grep my.cnf
```

mysql 的配置文件路径查找优先级为/etc/my.cnf,/etc/mysql/my.cnf,/usr/local/mysql/etc/my.cnf

通过 Homebrew 安装的 my.cnf 放在`/usr/local/mysql/etc中`

## 问题五

MySQL 查看和修改字符集方法

查看 MySQL 所支持的字符集

```
show charset;
```

查看库的字符集

```
show create database dbname \G;
```

查看表的字符集

```
show table status from hrs like 'tb_emp';
```

设置字符集

**创建库的时候指定字符集**

```sql
create database dbname default character set=utf8
```

**创建表的时候指定字符集**

```sql
create table tablename
(id INT(6), name CHAR(10))
default character set = 'utf8';
```

**修改库的字符集**

```sql
alter database dbname default character set utf8;
```

**修改表的字符集**

```sql
alter table tablename convert to character set utf8;
```

**查看全局字符集**

```sql
show variables like '%character%';
```

## 问题六

MySQL 日志配置

```
sudo mysql --help | grep my.cnf
```

登录 mysql，查看日志服务是否开启

```
show variables like 'log_bin';
```

创建文件并赋予权限

```bash
mkdir -p /var/run/mysqld

touch /var/run/mysqld/mysqld.pid

chown -R mysql:mysql /var/run/mysqld
```

**修改 my.cnf 文件**

```
log_bin = /usr/local/var/mysql/logs/mysql-bin
```

```
chown -R mysql:mysql /usr/local/var/mysql/logs
```

> 如果要修改 my.cnf 文件需要用
> sudo vim my.cnf
