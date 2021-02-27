---
title: 单点登录cas认证配置走过的坑
date: 2019-12-09 15:16:16
tags:
 - cas
 - 单点登录
categories:
 - cas
---
项目框架:spring+shiro+cas

1.使用shiro-cas包进行整合

	- realm认证需改为casRealm认证
	
	- CredentialsMatcher密码匹配器需使用自带密码匹配器，如果需要加密则自行添加相应密码匹配器，并对cas认证进行修改

	- shiroFilter配置中需添加casFilter到shiroFilter的filters过滤链中，切filterMap拦截配置中需加上对所有路径的casFilter拦截
	
*注意：使用此包认证token已变更为casToken，验证需要ticket凭证，不能通过shiro进行本地登录*

2.使用cas-client包进行整合

	- cas 和 shiro作为两个单独拦截器进行登录，通过访问域名的不同进行拦截登录

	- 先进行cas拦截，并重写AuthenticationFilter，在doFilter里对域名进行相应判断，cas登录后在进行shiro的登录，保证subject里有shiro的凭证

*注意:需重写shiro的sessionId保存到浏览器中的cookie名称，保证和cas使用的jsessionId不重复，否则在使用时会导致shiro和cas之间循环重定向*

***参考链接：https://blog.csdn.net/changliangwl/article/details/51504522***