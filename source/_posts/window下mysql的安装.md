---
title: window下mysql的安装
date: 2019-06-15 10:42:49
tags:
 - mysql
 - window
categories: 
 - mysql 
---
1.官网下载mysql压缩包(以5.7.26版本为例)
	
	https://dev.mysql.com/downloads/mysql/5.7.html#downloads

2.解压后进入文件根目录下

修改my_default.ini为my.ini，如果没有创建my.ini

备注：my.ini中内容: utf8mb4包含utf8及部分移动端的表情数据，basedir为mysql安装目录，datadir为mysql数据目录，该目录下数据必须为空否则安装会失败

	[client]
	port=3306
	default-character-set = utf8mb4

	[mysql]
	default-character-set = utf8mb4

	[mysqld]
	basedir = D:\mysql\mysql5.7.26
	datadir = D:\mysql\mysql5.7.26\data
	port = 3306
	character-set-client-handshake = FALSE
	character-set-server = utf8mb4
	collation-server = utf8mb4_unicode_ci
	init_connect = 'SET NAMES utf8mb4'
	sql_mode = NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES

3.配置mysql的系统环境变量

进入我的电脑->高级系统设置->添加系统变量，变量名：MYSQL_HOME 值：mysql安装目录

	编辑Path环境变量，添加 %MYSQL_HOME%\bin

4.使用cmd进入当前目录下的命令控制台

	执行mysqld --initialize --user=mysql --console

自动生成初始化数据库，执行完毕会生成root用户和临时密码(root@localhost后跟的随机字符串就是临时密码)
注意:如果第一次数据库初始化失败，第二次进行初始化的时候需要清空data中的文件，否则将不能进行初始化。

5.安装服务(MySQL为服务名称，文件路径为上文所创建ini配置文件)

	输入命令 mysqld install MySQL --defaults-file="D:\MySQL\mysql-5.7.25-winx64\my.ini

6.启动与关闭

	mysql启动:net start MySQL
	mysql关闭:net stop MySQL

7.进入mysql
	
	mysql -u root -p 使用root用户连接mysql输入密码后进入

第一次进入需要修改root用户密码才能使用sql语句进行调用
修改密码指令：set password = password('新密码');
执行成后使用exit退出，再次登录即可使用新密码