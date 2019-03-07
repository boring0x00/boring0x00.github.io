---
layout: post
title: 格式化字符串
author: "bo"
---

### pwn的学习笔记

##### 格式化字符串漏洞

1. 格式化字符串 是一些编程语言 在格式化输出API函数中制定输出参数的格式与相对位置的字符串参数。   下面以c语言的printf函数为例子。简单的说一下
    1. 格式化占位符指明输出参数如何格式化。语法是
```
%[parameter][flags][field width][.precision][length]type
```
    parameter可以忽略

    flag大致有这几类

| 字符      | 描述    |
| --------   | -----------:   |
| 香蕉        | $1      |
| 苹果        | $1      |
| 草莓        | $1      |
