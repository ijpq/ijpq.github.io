---
layout: post
title: shell - rm work with find
comments: true
tags: [shell]
toc: true
---

# 情况:

![目录情况](/assets/Snipaste_2020-10-09_10-52-22.png)

目录中含有大量*_$NUM.pkl的文件, NUM的范围1到119

需要执行的操作为仅保留所有119的文件,其他全部删除


1. 先使用find 将不包含119的文件筛选出来

```
find /content/drive/My\ Drive/datasets/HOREID/results_70clean/models -not -name '119'
```

![结果1](/assets/Snipaste_2020-10-09_10-56-05.png)

2. 配合rm -rf $()

```
rm -rf $(find /content/drive/My\ Drive/datasets/HOREID/results_70clean/models -not -name '119')
```
或者将find的结果传递给rm
```
find /content/drive/My\ Drive/datasets/HOREID/results_70clean/models -not -name '119' |xargs rm -rf
```


