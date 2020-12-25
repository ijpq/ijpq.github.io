---
layout: post
title: dynamic link printf instead of include
comments: true
toc: true
tags: [link]
---

we could dynamic symbol `printf` with libc.so instead of include<stdio.h> in source file.

*a.c*
```c
int main(){
	printf("hi! .so success!\n");
return 0;}	
```

`gcc a.c -lc`


a.c: In function ‘main’:

a.c:2:2: warning: implicit declaration of function ‘printf’ 

[-Wimplicit-function-declaration]

2 |  printf("hi! .so success!\n");

|  ^~~~~~

a.c:2:2: warning: incompatible implicit declaration of built-in 
function ‘printf’

a.c:1:1: note: include ‘<stdio.h>’ or provide a declaration of ‘printf’

  +++ |+#include <stdio.h>

1 | int main(){

`./a.out`

hi! .so success!

