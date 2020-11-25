---
layout: post
title: c++ 关于初始化的一切
comments: true
toc: true
tags: [initialization, assignment, default initialize, uninitialization, undefined]
---


看这个就够了,专业的

[cppreference](https://en.cppreference.com/w/cpp/language/initialization)
# 参考
[知乎回答](https://www.zhihu.com/question/36735960)



<!-- 
# 初始化和赋值的定义
`初始化` : `initialization`

`赋值` : `assignment`

初始化的定义:变量在创建的同时被赋值
赋值的定义:冲刷掉对象现有的值,替换为新的值

# 初始化

## 列表初始化
`列表初始化` : `list initialization`
```c++
int a = 0;
int a = {0};
int a{0};//列表初始化
int a(0);
```

## 默认初始化
`默认初始化` : `default initialization`

`未初始化` : `uninitialization`

`未定义` : `undefined`

定义:仅定义变量但不赋值时,执行默认初始化,这时取一个`默认值`.

`内建类型的变量`的默认值分两种情况,函数体外的默认值为0,函数体内的默认值是`未初始化`,这个值是`未定义的`

## 拷贝初始化
`拷贝初始化` : `copy initialization`

`直接初始化` : `direct initialization`

定义:当使用`=`初始化一个变量时,要求编译器对左侧对象进行拷贝初始化

当不使用`=`时,执行的是`直接初始化`

## 值初始化
`值初始化` : `value initialization`

这个概念是和容器相关的,如果只提供数量,不提供值,则会执行`值初始化`

如果容器的元素是类类型,则执行类的默认初始化
如果是内置类型,则一般取内建类型的默认值,比如int的0 -->
