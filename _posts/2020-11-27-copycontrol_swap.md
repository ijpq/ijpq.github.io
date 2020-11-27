---
layout: post
title: swap in chapter13 c++ primer
comments: true
toc: true
tags: [swap, copy control]
---

# 概念引入

swap函数主要用于对象重排序的算法,比如我们自己写的strVec类，实现了动态内存管理。同时strVec类明显需要排序算法，因此在排序过程中需要调用swap，但如果使用std::swap可能无法有效地对对象进行swap，所以需要重载swap。

## 前置1

[c++primer] : `定义swap的类通常使用swap来定义他们的赋值运算符`,这种操作称为`copy and swap`.

example code
```c++
HasPtr& HasPtr::operator=(HasPtr rhs){
    swap(*this, rhs);
    return *this;
}


HasPtr p1;
HasPtr p2 = p1;
```
形参rhs是实参p1的拷贝, swap的时候会将this和p1的拷贝交换.因此在函数返回时, this指向p1的拷贝,而原本this的内存被释放

所以最终实现了p1向p2的拷贝

## 前置2
对于既有copy constructor，也有move constructor的类，`右值移动，左值拷贝`

但对于只有copy constructor的类，右值也拷贝，原理如下

```c++
class A{
    A(const A&); //copy
    // no move
};
A a1;
A a2(a1);
A a3(std::move(a1));
```
因为a1是一个左值,所以a2是拷贝构造.

但std::move()应该返回右值引用.而且定义copy,不定义move时，编译器不会合成move.
由于绑定到右值表达式的可以是1.右值引用,2.const左值引用.所以A的copy可以匹配std::move()返回的右值引用A&&.相当于`const A& = A&& a1`

## 结果
最终我们发现，在`需要定义swap操作`的类，如果这个类还`定义move constructor`，则可以通过这种使用swap的赋值运算符同时实现`copy overlod`和`move overload`.

example code
```c++
HasPtr::HasPtr(const HasPtr& rhs){
    //copy
}
HasPtr::HasPtr(HasPtr&& rhs) noexcept {
    //move
}
HasPtr& HasPtr::operator=(HasPtr rhs){
    swap(*this, rhs);
    return *this;
}

HasPtr hp2;
HasPtr hp = hp2;
HasPtr hp3 = std::move(hp2);
```
在这种情况下，由于赋值运算符的参数是非引用类型，因此需要进行拷贝初始化.而拷贝初始化要不使用copy constructor,要不使用move constructor.

第一个赋值中， hp2是左值，因此使用copy constructor来进行拷贝初始化

第二个赋值中，std::move返回右值引用，因此使用move constructor进行拷贝初始化


