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
    ~Point(){
        cout<<"deconstructor"<<endl;
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
#include<vector>
using namespace std;

Point foo_bar2(Point&);

Point global;
Point foo_bar(Point arg){
    cout<<"Point local = arg"<<endl;
    Point local = arg; 

    cout<<"local = arg"<<endl;
    local = arg;

    cout<<"foo_bar2(Point&)"<<endl;
    foo_bar2(local);

    cout<<"vector.push_back(Point)"<<endl;
    vector<Point> vec;
    vec.push_back(local);

    cout<<"new Point(global)"<<endl;
    Point *ptr = new Point(global);

    cout<<"*ptr = local"<<endl;
    *ptr = local;

    cout<<"Point pa[4]"<<endl;
    Point pa[4] = {local, *ptr};

    cout<<"Point* ps = new Point"<<endl;
    Point* ps = new Point();

    cout<<"delete ps"<<endl;
    delete ps;
    
    cout<<"return *ptr"<<endl;
    return *ptr;
}
Point foo_bar2(Point& arg){
    return arg;
}

int main(){
    int x = 5;
    Point p1(x);
    Point p2 = p1;
    cout<<"calling foo_bar(Point)"<<endl;
    foo_bar(p1);
    return 0;
}
```

## output
```c++
default constructor
calling constructor

value of this->data:0
calling copy constructor
after calling: this->data:5

calling foo_bar(Point)
value of this->data:0
calling copy constructor
after calling: this->data:5

Point local = arg
value of this->data:0
calling copy constructor
after calling: this->data:5

local = arg
copy overload

foo_bar2(Point&)
value of this->data:0
calling copy constructor
after calling: this->data:5

deconstructor

vector.push_back(Point)
value of this->data:0
calling copy constructor
after calling: this->data:5

new Point(global)
value of this->data:0
calling copy constructor
after calling: this->data:10

*ptr = local
copy overload

Point pa[4]
value of this->data:0
calling copy constructor
after calling: this->data:5
value of this->data:0
calling copy constructor
after calling: this->data:10
default constructor
default constructor

Point* ps = new Point
default constructor

delete ps
deconstructor

return *ptr
value of this->data:0
calling copy constructor
after calling: this->data:10

deconstructor
deconstructor
deconstructor
deconstructor
deconstructor
deconstructor
deconstructor
deconstructor
deconstructor
deconstructor
deconstructor
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
