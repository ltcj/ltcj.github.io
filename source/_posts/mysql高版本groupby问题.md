---
title: mysql高版本groupby问题
date: 2019-01-03 09:29:19
tags:
- mysql
- groupby
categories:
- mysql
---
在mysql高版本中查询非groupby聚合列会出异常，此时需要修改mysql的sql_mode

1. 查询全局sql_mode
    
    select @@global.sql_mode;

2. 修改sql_mode为非only\_full\_group\_by模式(对当前会话、已存在数据库有效)

    set sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));

3. 修改全局sql_mode(对当前会话、新建数据库有效)

    set @@global.sql_mode ='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
	
	注意：执行不成功可能由于复制时中英文单引号错乱导致

4. 修改mysql的my.ini,添加以上配置(liunx为my.cnf,重启数据库后永久生效)