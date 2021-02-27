---
title: Git+Hexo更新博客简单指令
date: 2018-12-18 13:56:28
tags: 
 - hexo
 - git
 - 博客
categories: 
 - 博客
---
1.更新Git上hexo的配置文件(如果Git上已经创建配置文件分支则跳过前三步操作)

* git初始化

        git init

* 添加仓库地址
   
        git remote add origin https://github.com/用户名/仓库名.git

* 新建分支并切换到新建的分支

        git checkout -b 分支名

* 添加所有本地文件到git
    
        git add .

* git提交

        git commit -m "注释信息"

* 文件推送到hexo分支

        git push origin hexo

*注意：如果提交更新后存在文件未更新，可先清除缓存后再次重复提交操作：git rm -r --cached "文件夹的名称"*

2.更新hexo静态访问页面

* 清除上次生成文件
    
        hexo clean

* 生成新文件
    
        hexo g

* 部署新的文件
    
        hexo d

* 本地启动hexo服务器
		
		hexo server

3创建新的hexo资源

* 创建指定名称文章，后缀为.md

        hexo new "postName"

* 创建一个新的页签文件夹，里面包含一个index.md
    
        hexo new page "页签名称"
