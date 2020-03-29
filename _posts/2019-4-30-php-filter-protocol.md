---
layout: post
title: 
subtitle: "基础内容"
author: "bo"
header-img: "img/post-bg-web.jpg"
header-mask: 0.3
---
### php://filter伪协议妙用
#### 1. 基本介绍
>    1. php://filter 是一种设计用来允许过滤器程序在打开时成为流的封装协议。这对于单独具有完整功能的文件函数例如 readfile()，file() 和 file_get_contents() 很有用，否则就没有机会在读取内容之前将过滤器应用于流之上 
>    2. php://filter:/<action>=<name> 
>    3. 例子：
>    
#### 2. 