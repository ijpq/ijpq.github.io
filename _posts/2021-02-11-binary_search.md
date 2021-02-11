---
layout: post
title: 二分查找专题
comments: true
toc: true
tags: [c++, 二分查找]
---

## 问题定义

在区间[lo, hi)查找元素e

search语义约定：
如果成功，返回e的位置；如果失败，返回不大于e的最大的一个元素位置

**只有版本C符合语义约定**

**版本A到版本B的改进是为了平衡向左和向右走时的查找次数，使得在`最坏情况`下查找次数得到改善，而最好情况下版本A还是最好的（不过最好情况一般不会遇到）**

**版本B到版本C的改进是为了满足语义约定，因此最终版本C实现了`最坏情况下更少的查找次数`以及`search()的语义约定`**

## 版本A

循环条件：查找区间的元素至少有1个

判断条件：

**e < val[mid]**时向左, hi取mid；

**val[mid] < e**时向右,lo取mid+1；

**除此以外**返回当前位置

即[lo, mi) or [mid+1, hi) or mid hit

最终返回：表示查找失败，返回-1

### 实现

```c++
while (lo < hi) {
    mi = (lo+hi) >> 1;
    if (e < val[mid]) {
        hi = mi;
    } else if (val[mid] < e) {
        lo = mid + 1;
    } else {
        return mid;
    }
}
return -1;
```

## 版本B

循环条件：查找区间内元素至少有2个

判断条件：

**e < val[mid]**时向左,hi取mid; 

**val[mid] <= e**时向右, lo取mid.

即[lo, mi) or [mi, hi)

最终返回：如果**e == val[lo]**，返回lo；否则返回-1

## 版本C

循环条件：查找区间内元素至少有1个

判断条件: 

**e < val[mid]**时向左,hi取mid; 使得 e < [hi, n)

**val[mid]<=e**时向右，lo取mid+1; 使得 [0, lo) <= e

最终返回：--lo;

当区间元素个数缩减至0时，lo-1是不大于e( <= e )的最大元素


