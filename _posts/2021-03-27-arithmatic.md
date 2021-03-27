---
layout: post
title: 一些数学运算 
comments: true
toc: true
tags: [cpp, c++, C, mod, XOR]
---

# %
`x%y`语义上：如果x和y都是正整数，结果是x除以y的余数

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

[参考](https://zh.wikipedia.org/wiki/%E6%A8%A1%E9%99%A4)

如果看过了参考，就知道，上面这个示例代码所使用的编译器，对于符号模问题的实现是使余数与被除数同符号

此外,`参考`中的这个部分讲述得非常本质

---
几乎所有的计算系统中，a 除 n 得到商 q 和余数 r 均满足以下式子：

$\begin{aligned}
q & \in \mathbb{Z} \\
a &=n q+r \\
|r| &<|n|
\end{aligned}$up