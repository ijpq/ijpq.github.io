---
layout: post
title: c++ 关于初始化的一切
comments: true
toc: true
tags: [initialization, assignment, default initialize, uninitialization, undefined]
---


看这个就够了,专业的

[cppreference](https://en.cppreference.com/w/cpp/language/initialization)

[static, dynamic, automatic, thread_local](https://en.cppreference.com/w/cpp/language/storage_duration)

# cppreference
初始化一共分为以下几种:
1. Value initialization, e.g. std::string s{};
2. Direct initialization, e.g. std::string s("hello");
3. Copy initialization, e.g. std::string s = "hello";
4. List initialization, e.g. std::string s{'a', 'b', 'c'};
5. Aggregate initialization, e.g. char a[3] = {'a', 'b'};
6. Reference initialization, e.g. char& c = a[0];
7. If no initializer is provided, the rules of default initialization apply.

## default initialization
This is the initialization performed when a variable is constructed with no initializer.

automatic, static, thread_local storage duration with `no initializer`
dynamic with `new` with `no initializer`

class type - default constructor
elements in array - default initialized.
non class - automatic or dynamic - indeterminate
non class - static or thread local - zero initialized.

### storage duration
#### automatic
The storage for the object is allocated at the beginning of the enclosing code block and deallocated at the end
#### static
allocated when the program begins and deallocated when the program ends.
#### thread_local
 thread begins and deallocated when the thread ends.
#### dynamic
allocated and deallocated per request by using dynamic memory allocation functions

特点是`没有括号`

## value initialization
This is the initialization performed when a variable is constructed with an `empty initializer`.

value initial的特点是空的括号,比如`()`,`{}`

example:
```c++
T()	(1)	
new T ()	(2)	
Class::Class(...) : member() { ... }	(3)	
T object {};	(4)	(since C++11)
T{}	(5)	(since C++11)
new T {}	(6)	(since C++11)
Class::Class(...) : member{} { ... }	(7)	(since C++11)
```

class type - default initialization
class type - default constructor, zero-initialized and default initialized.
other - zero-initialized.

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
