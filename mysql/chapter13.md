# MySQL 用户管理

MySQL 是一个多用户数据库，具有功能强大的访问控制系统，可以为不同用户指定允许的权限。MySQL 用户
可以分为普通用户和 root 用户。root 用户是超级管理员，拥有所有权限，包括创建用户、删除用户和修改用户
的密码等管理权限，普通用户只拥有被授予各种权限。用户管理包括管理用户账户、权限等

### 权限表

MySQL 服务器通过权限表来控制用户对数据库的访问，权限表存放在 mysql 数据库中，由 mysql_install_db 脚本
初始化。存储账户权限信息表主要有 user、db、host、tables_priv、columns_priv、procs_priv

**user 表**

user 表是 MySQL 中最重要的一个权限表，记录允许连接到服务器的账号信息，里面的权限是全局级的。

例如，一个用户在 user 表中被授予了`DELETE`权限，则该用户可以删除 MySQL 服务器上所有数据库中的任何记录

- 用户列

user 表的用户列包括 Host、User、Password，分别表示主机名、用户名和密码。其中 User 和 Host 为 User 表的
联合主键。当用户与服务器之间建立连接时，输入的账户信息中的用户名称、主机名和密码必须匹配 user 表中对应的字段，
只有 3 个值都匹配的时候，才允许连接的建立

- 权限列

权限列的字段决定了用户的权限，描述了在全局范围内允许对数据和数据库进行的操作。包括查询权限、修改权限等普通权限，
还包括了关闭服务器、超级权限和加载用户等高级权限。普通权限用于操作数据库，高级权限用于数据库管理

- 安全列

安全列只有 6 个字段，其中两个是 ssl 相关的，2 个是 x509 相关的，另外 2 个授权插件相关的

- 资源控制列

max_questions 用户每小时允许执行的查询操作次数

max_updates 用户每小时允许执行的更新操作次数

max_connections 用户每小时允许执行的连接操作次数

max_user_connections 用户允许同时建立的连接次数

**db 表和 host 表**

db 表和 host 表是 MySQL 数据中非常重要的权限表，db 表中存储了用户对某个数据库的操作权限，决定用户能从
哪个主机存取那个数据库。
host 表中存储了某个主机对数据可的操作权限，配合 db 权限表对给定主机上的数据库操作权限做更细致的控制

**tables_priv 表和 columns_priv 表**

tables_priv 表用来对表设置操作权限
columns_priv 表用来对表的某一列设置权限

tables_priv 表有 8 个字段，Host、Db、User、Table_name、Grantor、Timestamp、Table_priv、Column_priv

Host、Db、User、Table_name 分别是主机名、数据库名、用户名和表名
Grantor 表示修改该记录的用户
Timestamp 字段表示修改该记录的时间
Table_priv 表示对表的操作权限包括 Select、Insert、Update、Delete、Create、Drop、Grant、Reference、Index、Alter
Column_priv 字段表示对表中的列的操作权限，包括 Select、Insert、Update、References、

columns_priv 只有 7 个字段，分别是 Host、Db、User、Table_name、Column_name、Timestamp、Column_priv。其中
Column_name 用来指定对哪些数据列具有操作权限

**procs_priv 表**

procs_priv 表可以对存储过程和存储函数设置操作权限

## 账户管理

MySQL 提供了许多语句用来管理用户账户，这些语句可以用来管理包括登录和退出 MySQL 服务器、创建用户
删除用户、密码管理和权限管理等内容。MySQL 数据库的安全性，需要通过账户管理保证

### 登录和退出 MySQL 服务器

查看 mysql 命令的常用参数

- `-h` 主机名，可以使用该参数指定主机名或 ip，如果不指定，默认是 localhost

- `-u` 用户名，可以使用该参数指定用户名

- `-p` 密码，可以使用该参数指定登录密码。如果该参数后面的有一段字符，则该段字符串将作为用户的登录密码直接登录。
  如果后面没有内容，则登录的时候会提示输入密码。注意该参数后面的字符串和-p 之间不能有空格

- `-P` 端口号，该参数后面接 MySQL 服务器的端口号，默认为 3306

- 数据库名，可以在命令的最后指定数据库名

- `-e` sql 语句，如果指定了该参数，将在登录后执行-e 后面的命令或 sql 语句并退出

可以使用双短线登录服务器

```
mysql --user=monty --password=guess db_name
```

较短的选项方式

```
mysql -uroot -p -hlocalhost test
```

登录服务器并执行 sql

```
mysql -uroot -p -hlocalhost test_db -e"DESC person"
```

**退出 MySQL**

```
exit;
```

### 新建普通用户

创建新用户，必须有相应的权限来执行创建操作。在 MySQL 数据库中，有两种方式创建新用户，一种是
使用`CREATE USER或GRANT`语句。另一种是直接操作 MySQL 授权表，最好的办法是使用 GRANT 语句，
因为这样更精确

```sql
CREATE USER user_specification [,user_specification]...
```

user_specification

```
user@host
[
  IDENTIFIED BY [PASSWORD] 'password' |
  IDENTIFIED WITH auth_plugin [AS 'auth_string']
]
```

user 表示创建的用户的名称，host 表示允许登录的用户主机名称
IDENTIFIED BY 表示用来设置用户的密码，[PASSWORD]表示使用哈希值设置密码，该参数可选，
'password'表示用户登录时使用的普通明文密码

CREATE USER 语句会添加一个新的 MySQL 账户，使用 CREATE USER 语句用户，必须有全局的
CREATE USER 权限或 MySQL 数据库的 INSERT 权限，每添加一个用户，CREATE USER 语句会在
`mysql.user`表中添加一条新记录，但是新创建的账户没有任何权限，如果添加的账户已经存在，
则 CREATE USER 语句会返回一个错误

示例

```sql
CREATE USER 'jeffrey'@'localhost' IDENTIFIED BY 'mypass';
```

如果只指定用户部分`jeffrey`，主机名部分默认为`%`(即对所有的主机开放权限)，user_specification
告诉 MySQL 服务器当用户登录时怎么验证用户的登录授权。如果指定用户登录时不需要密码，可以省略 IDENTIFIED
BY 部分

```sql
CREATE USER 'jeffrey'@'localhost'
```

此种情况下，MySQL 服务端使用内建的身份验证机制，用户登录时不能指定密码

如果要创建指定密码的用户，需要 IDENTIFIED BY 指定明文密码值，此种情况，用户登录时必须
指定密码

为了避免指定明文密码，如果知道密码的三列值，可以通过 PASSWORD 关键字使用密码的哈希值设置密码
哈希值可以使用 password()函数获取

```sql
SELECT password('mypass')
```

示例

```sql
CREATE USER 'jeffrey'@'localhost'
IDENTIFIED BY PASSWORD 'mypass的哈希三列值'
```

用户 jeffrey 的密码将被设置为 mypass

### 使用 GRANT 语句创建新用户

CREATE USER 语句可以用来创建账户，但是 CREATE USER 语句创建的新用户没有任何权限，还需要使用 GRANT 语句
赋予用户的权限。而 GRANT 语句不仅可以创建新用户，还可以在创建的同时对用户授权。

GRANT 还可以指定账户的其他特点，如果使用安全的连接、显示使用服务器资源等

GRANT 语句是添加新用户并授权他们访问 MySQL 对象的首选方法

```sql
GRANT privileges ON db.table
TO user@host [IDENTIFIED BY 'password'] [,user [IDENTIFIED BY 'password']]
[WITH GRANT OPTION]
```

privileges 表示赋予用户的权限类型，db.table 表示用户的权限所作用的数据库中的表，IDENTIFIED BY
关键字用来设置密码，WITH GRANT OPTION 为可选参数，表示对新建立的用户赋予 GRANT 权限，即用户可以对
其他用户赋予权限

示例，使用 GRANT 语句创建一个新的用户 testUser，密码为 testpwd，并授予用户对所有数据表的 SELECT 和
UPDATE 权限

```sql
GRANT SELECT,UPDATE ON *.* TO 'testUser'@'localhost'
IDENTIFIED BY 'testpwd';
```

> User 表中的 user 和 host 字段区分大小写，在查询的时候要指定正确的用户名称或着主机名

### 删除普通用户

在 MySQL 数据库中，可以使用 DROP USER 语句删除用户，也可以直接通过 DELETE 从`mysql.user`表中删除
对应的记录来删除用户

```sql
DROP USER user [,user];
```

示例

```sql
DROP USER 'jeffrey'@'localhost'
```

### root 用户修改自己的密码

root 用户的安全对于保证 MySQL 的安全非常重要，因为 root 用户拥有很高的权限

**使用 mysqladmin 命令在命令行指定新密码**

```
mysqladmin -u username -h localhost -p password 'newpwd'
```

username 为要修改密码的用户名，这里指 root
参数-h 指需要修改的、对应哪个主机用户的密码，该参数可以不写，默认是`localhost`

-p 表示输入当前密码

password 为关键字，后面引号的内容为新设置的密码。执行完此语句，roor 用户的密码将被
修改为 newpwd

### 使用 SET 语句修改 root 用户的密码

SET PASSWORD 语句可以用来重新设置其他用户的登录密码或者自己使用的账号的密码

```
mysql> SET PASSWORD=PASSWORD('rootpwd')
```

新密码必须使用 PASSWORD()函数加密

### root 用户修改普通用户密码

root 用户拥有很高的权限，不仅可以修改自己的密码，还可以修改其他用户的密码

**使用 SET 语句修改普通用户的密码**

```sql
SET PASSWORD FOR 'user'@'localhost' = PASSWORD('somepassword');
```

只有 root 可以通过更新 mysql 数据库的用户来更改其他用户的密码。如果使用普通用户
修改可以省略 FOR 子句更改自己的密码

```sql
SET PASSWORD = PASSWORD('somepassword');
```

### 使用 GRANT 语句修改普通用户密码

可以在全局级别使用 GRANT USAGE 语句指定某个账户的密码，而不影响账户当前的权限，使用 GRANT 语句
修改密码，必须拥有 GRANT 权限，一般情况下最好使用该方法来指定或修改密码

```sql
GRANT USAGE ON *.* TO 'someuser'@'%' IDENTIFIED BY 'somepassword';
```

### root 用户密码丢失怎么解决

对于 root 用户密码丢失这种特殊情况，MySQL 实现了对应的处理机制，可以通过特殊方法登录到 MySQL 服务器，
然后在 root 用户下重新设置密码

**使用`--skip-grant-tables`选项启动 MySQL 服务**

以 skip-grant-tables 选项启动时，MySQL 服务器将不加载权限判断，任何用户都能访问数据库。
在 Windows 操作系统中，可以使用 mysqld 或 mysql-nt 来启动 MySQL 服务进程。如果 MySQL 的目录
已经添加到环境变量中，可以直接使用 mysqld、mysql-nt 命令启动 MySQL 服务。否则需要先在命令行下
切换到 MySQL 的 bin 目录

mysqld 命令

```
mysqld --skip-grant-tables
```

在 Linux 操作系统中，使用 mysqld_safe 来启动 MySQL 服务，也可以使用`/etc/init.d/mysql`命令来
启动 MySQL 服务

```
mysqld_safe --skip-grant-tables user=mysql
```

```
/etc/init.d/mysql start-mysqld --skip-grant-tables
```

启动 MySQL 服务后，就可以使用 root 用户登录了，打开一个命令行窗口，输入不加密码的登录命令

```
mysql -u root
```

登录成功后，可以使用 UPDATE 语句或者使用 mysqladmin 命令重新设置 root 密码

```sql
UPDATE mysql.user SET password=PASSWORD('newpwd') WHERE User='root' AND Host='localhost'
```

### 加载权限表

修改密码完成后，必须加载权限表。加载权限表后，新的密码才会生效，同时 MySQL 服务器开始权限验证

```
mysql> FLUSH PRIVILEGES;
```

## 权限管理

权限管理主要是对登录到 MySQL 的用户进行权限验证。所有用户的权限都存储在 MySQL 的权限表中，
不合理的权限规划会给 MySQL 服务器带来安全隐患。数据库管理员需要对所有的用户的权限进行合理规划
管理。MySQL 权限系统的主要功能是证实连接到一台给定主机的用户，并且赋予该用户在数据库上的
SELECT、INSERT、UPDATE、DELETE 权限

### MySQL 的各种权限

账户权限信息被存储在 MySQL 数据库的 user、db、host、tables_priv、columns_priv、procs_priv 表中。
在 MySQL 启动时，服务器将这些数据库表中权限信息的内容读入内存

- CREATE 和 DROP 权限，可以创建新数据库和表，或删除已经有数据库和表，如果将 MySQL 数据库中的 DROP 权限授予
  某用户，用户可以删掉 MySQL 访问权限保存的的数据库

- SELECT、INSET、UPDATE、DELETE 权限允许在一个数据库现有的表上实施操作

- SELECT 权限只有在它们真正从一个表中检索行时才被用到

- INDEX 权限允许创建或删除索引，INDEX 适用已有表。如果具有某个表的 CREATE 权限，可以在 CREATE TABLE 语句中包括索引定义

- ALTER 权限，可以使用 ALTER TABLE 来更改表的结构和重新命名表

- CREATE ROUTINE 权限来创建保存的程序（函数和程序），ALTER ROUTINE 权限用来更改和删除保存的程序，EXECUTE 权限用来执行
  保存的程序

- GRANT 权限允许授权给其他用户，可用于数据库、表和保存的程序

- FILE 权限给予用户使用 LOAD DATA INFILE 和 SELECT ...INTO OUTFILE 语句读或写服务器上的文件，任何被授予
  FILE 权限的用户都能读或写 MySQL 服务器上的任何文件

**其余权限用于管理性操作**

- reload 命令告诉服务器授权表重新读入内存，flush-privileges 是 reload 的同义词
  refresh 命令清空所有表并关闭/打开记录文件

- shutdown 命令关掉服务器。只能从 mysqladmin 发出命令

- processlist 命令显示在服务器内执行的线程信息（即其他账户相关的客户端执行的语句）。kill 命令杀死服务器线程，
  用户总是能显示或杀死自己的线程，但是需要 PROCESS 权限来显示或杀死其他用户和 SUPER 权限启动的线程

- kill 命令能用来终止其他用户或更改服务器的操作方式

## 授权

授权就是为某个用户授予权限，合理的授权可以保证数据库的安全。MySQL 中可以使用 GRANT 语句为用户授予权限

**全局层级**

全局权限适用于一个给定服务器中的所有数据库。这些权限存储在 mysql.user 表中

```sql
GRANT ALL ON *.*
-- 撤销
REVOKE ALL ON *.*
```

**数据库层级**

数据库权限适用于一个给定数据库中的所有目标，这些权限存储在 mysql.db 和 mysql.host 表中

```sql
GRANT ALL ON db_name.*
REVOKE ALL ON db_name.*
```

**表层级**

表权限适用于一个给定表中的所有列，这些权限存储在 mysql.tables_priv 表中

```sql
GRANT ALL ON db_name.tbl_name
REVOKE ALL ON db_name.tbl_name
```

**列层级**

列权限适用于一个给定表中的单一列，这些权限存储在 mysql.columns_priv 表中。

要使用 GRANT 或 REVOKE，必须拥有 GRANT OPTION 权限

GRANT 语法

```sql
GRANT priv_type ON [object_type] tables TO
user [IDENTIFIED BY [PASSWORD] 'password']
[WITH GRANT OPTION]
```

- GRANT OPTION 将自己的权限赋予其他用户

示例

```sql
GRANT SELECT,INSERT ON *.* TO 'grantuser'@'localhost'
IDENTIFIED BY 'grantpwd'
WITH GRANT OPTION;
```

被授予 GRANT 权限的用户可以登录 MySQL 并创建其他用户账户

## 收回权限

收回权限就是取消已经赋予用户的某些权限，收回用户不必要的权限可以在一定程度上保证系统的安全性。
MySQL 中使用 REVOKE 语句取消用户的某些权限，使用 REVOKE 收回权限之后，用户账户的记录将从
db、host、tables_priv、columns_priv 表中删除，但是用户账户记录仍然在 user 表中

删除 user 表中账户记录，使用 DROP USER 语句

在将用户账户从 user 表删除之前，应该收回相应用户的所有权限。REVOKE 语句有两种语法格式

**收回用户所有的权限**

此语法用于取消对于已命名的用户的所有全局层级、数据库层级、表层级和列层级

```sql
REVOKE ALL PRIVILEGES, GRANT OPTION
FROM 'user'@'host'
```

**收回指定的权限**

```sql
REVOKE priv_type[(columns)]
ON tables
FROM 'user'@'host'
```

示例

```sql
REVOKE UPDATE ON *.* FROM 'testUser'@'localhost'
```

## 查看权限

SHOW GRANTS 语句可以显示指定用户的权限信息，使用 SHOW GRANT 查看账户信息的基本语法

```sql
SHOW GRANTS FOR 'user'@'host'
```

如果要查看更详细的信息可以用

```sql
SELECT privileges_list FROM user WHERE user='username', host='hostname';
```

## 访问控制

- 连接核实阶段

- 请求核实阶段

### 已经将一个账户的信息从数据库中完全删除，为什么该用户还能登录数据库

出现这种情况的原因可能有多种，最有可能的是 user 数据表中存在匿名账户，在 user 表中匿名账户的
User 字符串为空字符串，这会允许任何人连接到数据库，检测是否存在匿名登录用户的方法

```sql
SELECT * FROM mysql.user WHERE User='';
```

如果有记录返回，则说明存在匿名用户，需要删除该记录，以保证数据库的访问安全

```sql
DELETE FROM mysql.user WHERE User='';
```
