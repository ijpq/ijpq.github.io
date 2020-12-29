---
layout: post
title: fscanf with while loop
comments: true
toc: true
tags: [fscanf, read file, fclose]
---

assume `$num` is the number what you assign through scanf or fscanf or sscanf and so on.
```cpp
while (fscanf(fileptr, formatstring, &var1, &var2,...)!=$num) {
    //do some thing.      
}
fclose(fileptr);
```

if you encounter invalid pointer, check memory and pointer operation anywhere first.
because i have written this template but still stuck with `invalid pointer`. i found other pointer operation error finally.