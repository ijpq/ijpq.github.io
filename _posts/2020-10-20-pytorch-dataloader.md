---
layout: post
title: Pytorch - Dataset and Dataloader
comments: true
toc: true
tags: [Pytorch]
---

# 0x01
由于近期需要完成一个实验，在已有的模型上加入特殊的数据处理，即统计所有样本的loss并进一步筛选。因此需要自己写dataset和dataloader

# 0x02
## 预备知识

**Dataset**
Datasets有两种，分为map-style和iterable-style

iterable-style这种格式的不常用，官方给出的使用场景是`随机读取不可实现，batch_size不确定，需要根据提供的数据来决定,一般配合数据库读取和远程服务器读取数据使用`.一般我们是通过sampler抓取的index传入map-style的Dataset来获取数据

**map-style**
这种格式的dataset需要实现getitem和len,对样本建立了一个索引值index
定义好后，可以通过dataset[idx]得到样本数据和样本标签


**Dataloader**
dataloader默认使用一种通过整数index进行采样的sampler.如果index不是整数的，需要自定义sampler并传给dataloader
实际上不只是整数，我们需要读取的index都是有sampler决定的,默认是torch实现的batchsampler


**sampler**
torch已实现的sampler包含`sequentialsampler`,`randomsampler`,`weightedrandomsampelr`,`subsetrandomsampler`.

# 0x03
通过以上的分析可以知道，一般我们的做法都是写一个map的dataset，然后通过自定义sampler的方式来修改最终dataloder的方式

自定义sampler时需要注意的是
* 重载iter方法
* 重载len方法

## 已实现sampler分析
1. sequentialsampler
    ```python
    return iter(range(len(self.data_source)))
    ```
    所以顺序sampler就是把样本的所有index按照顺序返回出来
2. randomsampler
   ```
   n = len(self.data_source)
   return iter(torch.randperm(n, generator=self.generator).tolist())
   ```
3. weightedrandomsampler
   ```
   ```
4. subsetrandomsampler
   



>[参考](https://www.cnblogs.com/marsggbo/p/11308889.html)