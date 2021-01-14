---
layout: post
title: operating pointer to malloc
comments: true
toc: true
tags: [c,pointer,malloc,struct]
---

Assume that a piece of memory space is requested by **malloc**

```cpp
type *ptr = (type *) malloc(num * sizeof(element));
```

the ptr is the first address of the piece of memory.
if we want to move pointer to the next element,
it can be done like this:
```cpp
ptr = ptr + 1;
```

the reason why plus **1** rather than **sizeof(type)** is **the pointer has type**.

more than that, 8bytes is the size of pointer rather the type which pointed by pointer.
therefore, adding 1 to pointer results in value of pointer increasing counterpart value to 
the type of pointer.