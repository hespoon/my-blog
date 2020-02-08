---
title: SQL 命令学习
date: 2020-01-30 20:41:24
tags:
---
# MySQL 命令
* 登录本机的 MySQL 数据库 
mysql -u root -p
* 登录 MySQL 服务器
mysql -h 主机名 -u 用户名 -p
-h：指定客户端需要登陆的 MySQL 主机名，登录本机时可以省略。
-u：登录的用户名
-p：告诉服务器将会使用一个密码登录。如果密码为空，可以忽略此选项。
* 创建新用户
MySQL 默认使用严格模式，禁止使用 INSERT 命令直接向 mysql 数据库的 user 表中插入新行。
```SQL
create user username@host indentified by 'password'; // 创建用户，没有任何权限
SHOW GRANTS FOR username@host; // 查询用户权限
```
username 和 host 用 @ 符号分割。host 指明用户登陆 MySQL 服务器的主机。host 为 % 时，表示用户可以从任何主机登陆 MySQL 服务器。
* 向用户授予权限
GRANT 语句
```SQL
GRANT privilege,[privilege],.. ON privilege_level 
TO 'username'@'localhost' [IDENTIFIED BY password]
[REQUIRE tsl_option]
[WITH [GRANT_OPTION | resource_option]];
```
[] 内的内容为可选选项。
GRANT 关键字之后可以指定一个或多个权限。权限之间以逗号分隔。
privilege_level 指定权限级别。MySQL 支持全局 `*.*` 权限，数据库 `database.*` 权限，表 `database.table` 权限和列权限。使用列权限时，必须在每个权限之后用逗号分隔列名。
放置要授予权限的用户。如果用户已存在，则 GRANT 语句修改其权限。如果不存在，则 GRANT 语句将创建一个新用户。可选的 IDENTIFIED BY 允许为新用户设置密码。
REQUIRE tsl_option 指定用户是否必须通过安全连接连接到数据库
WITH GRANT OPTION 子句允许此用户授予其他用户权限。
RESOURCE OPTION 子句用于分配 MySQL 服务器的资源。例如，设置用户每小时可以使用多少个连接或语句。
使用 GRANT 语句必须要有 GRANT OPTION 权限。如果启用了 read_only 系统变量，则需要具有 SUPER 权限才能执行 GRANT 语句。


