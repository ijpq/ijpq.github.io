---
layout: post
title: (整型,char)和(整型,string)的转换
comments: true
tags: [char, string, 类型转换]
toc: true
---


# 0x01 数据类型基础
c: char [], char * 两者区别见[stackoverflow](https://stackoverflow.com/questions/25653034/the-difference-between-char-and-char/25653168)

c++: string

# 0x02 转换方法
## int -> char

> ```
> char ch = '0' + 3
> ```

## char -> int
> ```
> int r = ch - '0'
> ```

## c字符串 -> 整型
atoi,atol,atoll....

## 整型 -> c字符串
sprintf

## c++字符串(string) -> 整型
stoi,stol,stoll,...

## 整型 -> c++字符串(string)
to_string



# 0x03 参考
[参考总结](https://blog.csdn.net/fcku_88/article/details/88298340) 




