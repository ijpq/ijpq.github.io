---
layout: post
title: 拷贝构造函数,拷贝运算符重载什么时候调用?以及移动构造函数和删除的函数之间关系
comments: true
tags: [c++ primer, homework, copy constructor, move constructor]
toc: true
---

# 基本定义及结构

## Point.h
```c++

#include <memory>

#include<iostream>
using namespace std;


class Point{

public:
    Point(const Point& p){
        cout<<"value of this->data:"<<this->data<<endl;
        cout<<"calling copy constructor"<<endl;
        this->data = p.data;
        cout<<"after calling: this->data:"<<this->data<<endl;
    }
    Point(Point&&) noexcept;
    // Point (Point&& pp) noexcept{
    //     cout<<"value of this->data:"<<this->data<<endl;
    //     cout<<"calling move constructor"<<endl;
    //     this->data = pp.data;
    //     cout<<"after calling: this->data:"<<this->data<<endl; 
    // }
    Point(){
        cout<<"default constructor"<<endl;
        this->data = 10; //default
    }
    Point(int d):data(d){
        cout<<"calling constructor"<<endl;
    }
    Point& operator=(Point && pmove) noexcept{
        cout<<"move overload"<<endl;
        return *this;
    }
    Point& operator=(const Point& pcopy){
        cout<<"copy overload"<<endl;
        return *this;
    }
    Point foo_bar(Point);
private:
    int data = 0;
};

inline Point::Point(Point&& pp) noexcept{
    cout<<"move"<<endl;
}
```

## Point.cpp
```c++

#include "Point.h"
#include <memory>
using namespace std;

Point global;
Point foo_bar(Point arg){
    cout<<"local = arg"<<endl;
    Point local = arg; 
    cout<<"new Point"<<endl;
    Point *ptr = new Point(global);
    cout<<"*ptr = local"<<endl;
    *ptr = local;
    cout<<"Point pa[4]"<<endl;
    Point pa[4] = {local, *ptr};
    cout<<"return *ptr"<<endl;
    return *ptr;
}

int main(){
    int x = 5;
    Point p1(x);
    Point p2 = p1;
    cout<<"calling foo_bar"<<endl;
    foo_bar(p1);
    return 0;
}
```

## output
```c++
default constructor // 全局变量 global的默认构造函数
calling constructor // 变量p1
// 变量p2
value of this->data:0 
calling copy constructor
after calling: this->data:5

calling foo_bar
value of this->data:0 // foo_bar的形参arg
calling copy constructor
after calling: this->data:5 // 把实参p1的值拷贝到形参arg

local = arg
value of this->data:0
calling copy constructor
after calling: this->data:5

new Point
value of this->data:0 // *ptr的初始化
calling copy constructor
after calling: this->data:10 //把global的值拷贝到*ptr

*ptr = local //拷贝重载
copy overload

Point pa[4] 
value of this->data:0 //数组的第一个元素local
calling copy constructor
after calling: this->data:5
value of this->data:0 //数组的第二个元素*ptr,因为之前已经被global拷贝,因此现在拷贝给数组第二个元素,使数组第二个元素的值为10
calling copy constructor
after calling: this->data:10

//这两个默认构造有待研究,应该和数组的内存分配有关系
default constructor
default constructor
//函数返回
return *ptr
value of this->data:0
calling copy constructor
after calling: this->data:10
```

# 调用分析

如输出的注释所见

# copy assignment operator is implicitly deleted问题

如果仅定义了拷贝构造函数和拷贝运算符重载,则在Point的操作中遇到bug
```c++
Point.cpp:13:10: error: object of type 'Point' cannot be assigned because its copy assignment
      operator is implicitly deleted
    *ptr = local;
         ^
./Point.h:17:5: note: copy assignment operator is implicitly deleted because 'Point' has a
      user-declared move constructor
    Point(Point&&) noexcept;
    ^
1 error generated.
```
原因如c++ primer 第476页所述.
