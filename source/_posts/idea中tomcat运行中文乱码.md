---
title: idea中tomcat运行中文乱码
date: 2018-12-18 10:07:47
tags:
 - idea
 - tomcat
categories:
 - idea
---
1.进入tomcat配置Run/Debug Configurations中，在VM options中填入-Dfile.encoding=UTF-8

![配置图一](1.png)

2.大部分情况配置一即可解决，如乱码问题未解决请继续配置二。进入tomcat配置Run/Debug Configurations中，进入Startup/Connection配置面板中，勾选Pass environment variables选项，并添加一个environment variables，Name填写JAVA_OPTINS,Value填-Dfile.encoding=UTF-8

![配置图二](2.jpg)

3.在idea2019中，需配置HELP->Edit Custom VM OPtions文件,在其中加上-Dfile.encoding=utf-8

![配置图三](3.png)