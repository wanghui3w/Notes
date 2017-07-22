# mysql安装
环境:
* 系统：windows10
* mysql：5.7.19

## 下载MySQL

## 解压并安装
### 解压至目录
```
D:\app\mysql-5.7.19-winx64
```
### 配置环境变量

```
键名：MYSQL_HOME
值为：D:\app\mysql-5.7.19-winx64
```
### 准备my.cnf文件
里面使用了%MYSQL_HOME%变量，有问题，所以改写了绝对路径；文件放置在主目录。
```
[client]
port=3306
default-character-set=utf8
[mysqld]
port=3306
character_set_server=utf8
basedir=D:\app\mysql-5.7.19-winx64
datadir=D:\app\mysql-5.7.19-winx64\data
[WinMySQLAdmin]
D:\app\mysql-5.7.19-winx64\bin\mysqld.exe
```
### 管理员身份执行cmd
```
mysqld.exe -install
mysqld --initialize-insecure --user=mysql #windows下不需要使用`--user`参数
net start mysql

```
### 修改密码

```
D:\app\mysql-5.7.19-winx64>mysqladmin -u root -p password
Enter password:
New password: *******
Confirm new password: *******
Warning: Since password will be sent to server in plain text, use ssl connection to ensure password safety.
```

### 登陆
```
D:\app\mysql-5.7.19-winx64>mysql -uroot -p
Enter password: *******
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 5.7.19 MySQL Community Server (GPL)

Copyright (c) 2000, 2017, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)

mysql> exit
```

### 删除数据库

如果不再想用mysql了，则可以执行如下命令：
```
mysqld --remove
```
