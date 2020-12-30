---
layout: post
title: All implementations of two-dimensional array parameter passing 
comments: true
toc: true
tags: [c, passing parameter, parameter, 2darray]
---

# IN C
gcc 5.4.1 c99
gdb 7.11.1
## 0X01
array bound are fully determined at compile time.
```c
#include <stdio.h>

#define R 3 
#define C 4

void func(int arr[R][C]) {
    for (int i=0; i <R;++i){
        for (int j =0; j<C;++j){
            arr[i][j] = i+j;
            printf("%d,", arr[i][j]);
        }
    }
    
}

int main()
{
    int arr[R][C] = {0};
    func(arr);

    return 0;
}
```

## 0x02
this is the first choice for dealing with 2darray. operating pointer to element of array and malloc with free should be considered.
```c
#include <stdio.h>
#include <stdlib.h>

void func(int* array, int rows, int cols)
{
  int i, j;

  for (i=0; i<rows; i++)
  {
    for (j=0; j<cols; j++)
    {
      array[i*cols+j]=i+j;
      printf("%d,", array[i*cols+j]);
    }
  }
}

int main()
{
  int rows = 3, cols = 4;
  // or you can call scanf assigning value to rows and cols.
  int *x;

  /* obtain values for rows & cols */

  /* allocate the array */
  x = malloc(rows * cols * sizeof(*x));

  /* use the array */
  func(x, rows, cols);

  /* deallocate the array */
  free(x);
}
```

## 0x03
there are other methods that impl 2darray, such as only denote the second dim of array A[][N]. but those methods are less practical.

in addition, impl this methods by declaring pointer to pointer is a bit of complicated.

_pointer to pointer code example as follow:_


```c
void func(int** array, int rows, int cols)
{
  int i, j;

  for (i=0; i<rows; i++)
  {
    for (j=0; j<cols; j++)
    {
      array[i][j] = i*j;
    }
  }
}

int main()
{
  int rows, cols, i;
  int **x;

  /* obtain values for rows & cols */

  /* allocate the array */
  x = malloc(rows * sizeof *x);
  for (i=0; i<rows; i++)
  {
    x[i] = malloc(cols * sizeof *x[i]);
  }

  /* use the array */
  func(x, rows, cols);

  /* deallocate the array */
  for (i=0; i<rows; i++)
  {
    free(x[i]);
  }
  free(x);
}
```