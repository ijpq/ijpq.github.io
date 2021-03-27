---
layout: post
title: 一些数学运算 
comments: true
toc: true
tags: [cpp, c++, C, mod, XOR]
---

# %
`x%y`语义上：如果x和y都是整数，结果是x除以y的余数

这个运算结果的取值范围是[0, x-1]


**但值得注意的是**
```c++
// Program to illustrate the
// working of the modulo operator

#include <stdio.h>

int main(void)
{

	// To store two integer values
	int x, y;

	// To store the result of
	// the modulo expression
	int result;

	x = -3;
	y = 4;
	result = x % y;
	printf("%d", result);

	x = 4;
	y = -2;
	result = x % y;
	printf("\n%d", result);

	x = -3;
	y = -4;
	result = x % y;
	printf("\n%d", result);

	return 0;
}
```
```
-3
0
-3
```