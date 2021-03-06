---
layout: post
title:  "biomarker的笔记"
date:   2020-11-06
categories: [memo]
---


# biomarker相关的信息.

biomarker指的是和生物的一些表性特征相关联的分子,
使用biomarker可以来预测和判断样本的状态, 比如诊断,预后等等.

从功能和分子类型可以把biomarker分成各种类型,这里不再赘述.

值得关注的是对biomarker的要求以及biomarker的一些特性

## 寻找biomarker的两种策略.

一种策略就是, 对于某种病症,我们根据已有的知识, 先选好一批和病理过程相关的分子.然后在这些分子中筛选部分作为biomarker, 这种被称为基于假设的策略的缺陷在于可选范围太小.

另一种方法就是不做假设, 直接从数据出发广撒网, 直接找那些和表型信息相关的分子.这种基于数据的策略得到的结果往往收到假阳性的困扰.

## 对biomarker的特性要求

区分能力的要求包括特异性(specificity)与敏感性(sensitivity),
这两个性质和常见的机器学习中的概念相同.

其次是重复性.也就是biomarker需要在更多的样本上得到同样的结果.

## 如何提高biomarker的质量.

总的来说,就是要多做验证

### 假阳性问题

多个biomarker组合起来作为一个新的biomarker, 可以缓解单分子biomarker的假阳性问题.
但组合起来的biomarker依然不能完全解决重复性.

另外就是要多用实验验证. 来降低假阳性

### 重复性问题

提高重复性的方法也很直接,就是多用不同来源的样本来验证biomarker的区分能力. 
