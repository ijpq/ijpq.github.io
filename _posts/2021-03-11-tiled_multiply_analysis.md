---
layout: post
title: tiled multiply analysis
comments: true
toc: true
tags: [tiled multiply, cuda]
---
假设A为3x4,B为4x3

physical structure

A[0,1,2,...,11];B[0,...,11]

logical structure

A[0,1,2,3]

A[4,5,6,7]

A[8,9,10,11]

B[0,1,2]

B[3,4,5]

B[6,7,8]

B[9,10,11]

---
**implv1**

`ph=0`

threadx=0,thready=0.

Mds[0][0] = A[0]
Nds[0][0]=B[0]

threadx=1,thready=0.

Mds[0][1] = A[1]
Nds[0][1] = B[1]

threadx=0,thready=1

Mds[1][0] = A[4] 
Nds[1][0] =B[3]

threadx=1,thready=1

Mds[1][1]=A[5]
Nds[1][1] = B[4]

在phase=0阶段，A[0][1][4][5]完成加载，B[0][1][3][4]完成加载
```
A
[A[0],A[1]]
[A[4],A[5]]
B
[B[0],B[1]]
[B[3],B[4]]
```

`ph=1`
threadx=0,thready=0
Mds[0][0]=A[2]
Nds[0][0]=B[6]

threadx=1,thready=0
Mds[0][1]=A[3]
Nds[0][1]=B[7]

threadx=0,thready=1
Mds[1][0]=A[4+2+0]=A[6]
Nds[1][0]=B[(2+1)*3+0]=B[9]

threadx=1,thready=1
Mds[1][1]=A[7]
Nds[1][1]=B[10]

phase=1阶段,A[2,3,6,7]完成加载，B[6,7,9,10]完成加载
```
A
[A[2],A[3]]
[A[6],A[7]]
B
[B[6],B[7]]
[B[9],B[10]]
```
**以上均符合矩阵乘法所需元素**

---
impl v1
```cpp
__global__ void MatrixMulKernel(float* M, float* N, float* P,
      int Width) {
1. __shared__ float Mds[TILE_WIDTH][TILE_WIDTH];
2. __shared__ float Nds[TILE_WIDTH][TILE_WIDTH];
3. int bx = blockIdx.x; int by = blockIdx.y;
4. int tx = threadIdx.x; int ty = threadIdx.y;
      // Identify the row and column of the P element to work on
5. int Row = by * TILE_WIDTH + ty;
6. int Col = bx * TILE_WIDTH + tx;
7. float Pvalue = 0;
// Loop over the M and N tiles required to compute P element
8. for (int ph = 0; ph < Width/TILE_WIDTH; ++ph) {
        // Collaborative loading of M and N tiles into shared memory
9.  Mds[ty][tx] = M[Row*Width + ph*TILE_WIDTH + tx];
10. Nds[ty][tx] = N[(ph*TILE_WIDTH + ty)*Width + Col];
11. __syncthreads();
12. for (int k = 0; k < TILE_WIDTH; ++k) {
13. Pvalue += Mds[ty][k] * Nds[k][tx];
}
14. __syncthreads();
}
15. P[Row*Width + Col] = Pvalue;
```
---
`phase=0`

threadx=0,thready=0

Ads[0][0]=A[0]
Bds[0][0]=B[0]

threadx=1,thready=0

Ads[0][1]=A[1]
Bds[1][0]=B[1*3]=B[3]

threadx=0,thready=1

Ads[1][0] = A[1*4]=A[4]
Bds[0][1] = B[1]

threadx=1,thready=1

Ads[1][1]=A[1*4+1]=A[5]
Bds[1][1]=B[1*3+1]=B[4]

phase=0后加载A[0,1,4,5] & B[0,3,1,4]
```
A
[A[0],A[1]]
[A[4],A[5]]
B
[B[0],B[1]]
[B[3],B[4]]
```

`phase=1`

threadx=0,thready=0

Ads[0][0]=A[2]
Bds[0][0]=B[2*3]=B[6]

threadx=1,thready=0

Ads[0][1]=A[2+1]=A[3]
Bds[1][0]=B[(2+1)*3]=B[9]

threadx=0,thready=1

Ads[1][0]=A[1*4+2]=A[6]
Bds[0][1]=B[(2)*3+1]=B[7]

threadx=1,thready=1

Ads[1][1]=A[4+2+1]=A[7]
Bds[1][1]=B[(2+1)*3+1]=B[10]

phase=1后加载A[2,3,6,7] & B[6,9,7,10]

```
A
[A[2],A[3]]
[A[6],A[7]]
B
[B[6],B[7]]
[B[9],B[10]]
```

impl v2
```cpp
    //@@ You have to use shared memory for this MP
    __shared__ float Ads[TILE_WIDTH][TILE_WIDTH];
    __shared__ float Bds[TILE_WIDTH][TILE_WIDTH];

    // blockDim.x = blockDim.x = TILE_WIDTH
    int row = blockIdx.y * TILE_WIDTH + threadIdx.y;
    int col = blockIdx.x * TILE_WIDTH + threadIdx.x;

    // XXX 
    float p_value = 0;
    for (int phase = 0; phase < (numAColumns/TILE_WIDTH) ; phase++) {
        Ads[threadIdx.y][threadIdx.x] = A[row*numAColumns + phase*TILE_WIDTH + threadIdx.x];
        Bds[threadIdx.x][threadIdx.y] = B[(phase*TILE_WIDTH+threadIdx.x)*numBColumns + row];
        __syncthreads(); 
        for (int k=0; k<TILE_WIDTH;k++) {
            p_value += Ads[threadIdx.y][k] * Bds[k][threadIdx.x]; 
        }
        __syncthreads();
    }
    C[row*numCColumns + col] = p_value;
```



**没问题啊，为什么最后所有测试数据没有一个能过的？？除非从两个点检查，1检查blockidx=1的情况，是否依然相同；2检查求和是否相同，写入内存的位置是否相同**