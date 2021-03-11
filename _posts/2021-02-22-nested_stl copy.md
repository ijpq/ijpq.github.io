---
layout: post
title: 理解嵌套容器
comments: true
toc: true
tags: [nested, nested stl, stl]
---

**source code**
```cpp
#include <stdio.h>
#include <unordered_map>
#include <string>
#include <iostream>
using namespace std;

int main()
{
    
    // printf("Hello World");
    
    unordered_map<string, unordered_map<string, int>> g;
    unordered_map<string, int> m = {{"b",1}};
    unordered_map<string, int> m2 = {{"b2",1}};
    g.emplace("a", m);
    cout << g["a"].size() <<endl; // 1
    g.emplace("a", m2);
    cout << g["a"].size() <<endl; // 1
    g["a"]["c"]= -1;
    cout << g["a"].size() <<endl; // 2
    g["a"].emplace("x1",1);
    cout << g["a"].size() <<endl; // 3
    g["a"].emplace("x2",1);
    cout << g["a"].size() <<endl; // 4
    g["a"].insert({"x3",1});
    cout << g["a"].size() <<endl; // 5

    return 0;
}

```

**output**
```
1                                                                                                                                                             
1                                                                                                                                                             
2                                                                                                                                                             
3                                                                                                                                                             
4                                                                                                                                                             
5                                                                                                                                                             
                                                                                                                                                              
                                                                                                                                                              
...Program finished with exit code 0                                                                                                                          
Press ENTER to exit console.  
```