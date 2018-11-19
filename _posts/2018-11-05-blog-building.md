---
layout: post
title: "Blog building"
date: 2018-11-05 18:00
author: "boring"
header-style: text
tags:
  - blog
  - github page
  - jekyll
---

## 前言
---
入坑一年多了，一直没有养成写博客的习惯，今天踩遍了坑，终于用github + jekyll搭建好了博客，这里感谢HUX大佬的主题，hihi
搭建过程。  
1. 先在本地下载好ruby  
2. 下载jekyll   
```
        gem gem install jekyll
        gem install bundler#这个是个打包器，会根据gemfile指明的依赖的东西都给你弄好
```
3. 创建博客  
```
        jekyll new blog #创建博客，此处也可以使用别人的主题
        #之后可以cd到博客目录
        jekyll server --watch #启动这个服务，这样能马上看到本地改动，之后能在127.0.0.1：4000看到预览

```
4. 提交到github仓库
```
        git status#查看状态
        git add .
        git commit "导入"
        git push -u origin master
```

   
5. 自己踩的坑： 
    1. 居然开了ssr，再去访问本机localhost，死活访问不上，所以去代理规则把这些去掉了。
    2. 补充一点，每次git push 都要输入账户密码，所以可以执行
```
                      git config --global credential.helper store
```  