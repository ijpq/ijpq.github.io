---
layout: post
title: C 字符串
comments: true
tags: [c/c++, string]
---

# 0x01
[参考内容，讲的很清楚](http://c.biancheng.net/cpp/html/80.html)

# 0x02
c的字符串有两种方式,分别为:
* char 数组
* char* 

第二种方式不支持字符串中具体字符的修改,这很像python的字符串特性.形成这个问题的原因是,字符数组存储在全局数据区,有读写权限;而char* 存储在常量区,不具备写权限,因此被称为字符串常量
