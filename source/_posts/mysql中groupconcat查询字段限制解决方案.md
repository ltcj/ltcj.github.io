---
title: mysql中groupconcat查询字段限制解决方案
date: 2019-01-03 09:29:19
tags:
- mysql
- group_concat
categories:
- mysql
---
mysql中使用group_concat时如果查询字段过长会被限制查询结果长度

1. 会话窗口一次性方案

	SET GLOBAL group_concat_max_len=1024000;  //需具有全局修改权限

	SET SESSION group_concat_max_len=1024000;

	备注：不需重启mysql服务

2. 彻底修改

	找到MYSQL的配置文件my.ini或者my.cnf,在里面添加以下信息
	
	group_concat_max_len = -1  # -1为最大值或填入你要的最大长度或者自己设置group_concat_max_len = 1024000

	备注:需重启mysql服务