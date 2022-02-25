# MySQL 安装与配置

## MySQL 停止与启动

### Linux

```
service mysql start

service mysql stop

service mysql restart
```

### mac

```
sudo /usr/local/mysql/support-files/mysql.server start

sudo /usr/local/mysql/support-files/mysql.server stop

sudo /usr/local/mysql/support-files/mysql.server restart
```

查看 mysql 进程

```
ps -e | grep mysql
```

杀死进程

```
kill -9 进程号
```
