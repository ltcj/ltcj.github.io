---
title: 单点登录相同顶级域名domain在高版本tomcat下抛异常
date: 2019-12-09 14:51:05
tags:
 - tomcat
 - 单点登录
categories:
 - tomcat
---
单点登录项目访问浏览器报错:An invalid domain [.zshs.tv] was specified for this cookie

原因：高版本tomcat(8.5)使用Rfc6265CookieProcessor生成cookie头，低版本使用LegacyCookieProcessor

解决方案：
	
一 .普通web项目

	1.进入tomcat文件目录

	2.Edit the Tomcat/conf/content.xml

	3.Add the statement in betweeen the <context> and </context> tags:

	<CookieProcessor className="org.apache.tomcat.util.http.LegacyCookieProcessor" /> 


二 .springboot项目

	@Bean
	public WebServerFactoryCustomizer<TomcatServletWebServerFactory> cookieProcessorCustomizer() {
	return (factory) -> factory.addContextCustomizers(
			(context) -> context.setCookieProcessor(new LegacyCookieProcessor()));
	}


备注：

	Rfc6265CookieProcessor 使用的domain规则如下：
	1、必须是1-9、a-z、A-Z、. 、- （注意是-不是_）这几个字符组成 
	2、必须是数字或字母开头 （所以以前的cookie的设置为.i5a6.com 的机制要改为 i5a6.com 即可） 
	3、必须是数字或字母结尾
