---
layout: post
title: Algorithm for understanding pointer in C
comments: true
toc: true
tags: [CMU15213, pointer]
---


1. find the variable name
2. translate the operator adjacent to the var name with prioirty
3. translate anthor symbol adjacent to the var in the opposite direction
4. go back to step 2
5. finally, deal with the class declarition such as int and so forth


```c++
// only a * on the left side adjacent to p, there is no symbol on the right side
int *p // 1

// * on the left side, [] on the right side. but [] is prior to *.
// the order is p -> p[13] -> int *(p[13])
int *p[13] // 2

// this order is constrant by ()
int *(p[13]) // 3

// p -> *p -> int * *p
int **p // 4 


int (*p)[13] // 5

// () is prior to *
// f is a function which returns a pointer to int
// f -> f() -> int *(f())
int *f() // 6

// f is a pointer for the priority of *
// and then, the pointer point to function
// f -> *f -> *f() -> int (*f())
int (*f)() // 7

// f is a function which returns a pointer.
// the pointer point to a array of 13 elements.
// each element is a pointer which point to a function which return int.
int (*(*f())[13])() // 8

// x is a array of which each is a pointer.
// the pointer point to a function which return pointer
// that point to a array of int
int (*(*x[3])())[5] // 9
```
---
[other refs](https://www.geeksforgeeks.org/dynamically-allocate-2d-array-c/)