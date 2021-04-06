---
layout: post
title:  "DESeq2 模型的交互项 "
date:   202-11-21
categories: [memo, bioconductor]
---


## 关于DESeq2 模型的交互项

## 线性模型的交互项 

DESeq2的交互项来自于它的线性模型.而它的模型设置也沿用了R中线性模型的特点.
所以为了解释DESeq2的交互项, 先了解一下R中线性模型的交互项是有必要的.

考虑最简单的两个自变量的线性模型, 不带有交互项的. 

\( y=\beta_1 *x_1 + \beta_2 * x_2 + \epsilon \)

可以使用偏导数来解释 \(x_1\) \(x_2\) 对y的影响:

\(\frac{\partial y}{\partial x_1} = \beta_1\)

\(\frac{\partial y}{\partial x_2} = \beta_2\)

此时 \(x_1\) 与\(x_2\) 是相互独立的,也就是互不干扰.

如果 \(x_1\) 与\(x_2\) 存在相互作用, 我们考虑一个简单交互项情况:

\( y = \beta_1 * x_1 + \beta_2 * x_2 + \beta_3*x_1*x_2 + \epsilon \)

添加了一个系数是\(\beta_3\)的交互项.

此时,我们再次用偏微分描述\(x_1\) 对 \(y\) 影响时, 表达式变成了:

\(\frac{\partial y}{\partial x_1}= \beta_1 + \beta_3*x_2\)

此时,\(x_1\)对\(y\)的影响是没有交互项时候的 \(\beta_2\) 加上一个随着\(x_2\)变化的部分.

在接下来的DESeq部分中,我们能够体会到这种相似性.

## DESeq2的模型

在讨论DESeq2的交互项相关的内容之前,了解DESeq2的结果代表了什么是比较重要的.

差异表达,顾名思义, 是要两种不同的样本A和B之间来比较基因的表达水平, 
那么表达水平的fold change(比值) 就要确定分子是什么,分母是什么.

这就涉及到reference level 这个概念.

DESeq2会默认用来分类的因子变量的第一个level作为reference level,也就是作为分母.

而用户不对因子变量作处理,因子变量的level 会默认按照字母序排列.

### 最简单的单因素

为了说明这一点我们先生成一个最简单的DESeq处理范例:

```R 建立最简单的DESeq实例
library(DESeq2)
dds <- makeExampleDESeqDataSet(n=10000,m=6)

```

此时使用 `colData(dds)` 可以得到如下结果.

``` bash
## DataFrame with 6 rows and 1 column
##         condition 
##          <factor> 
## sample1         A 
## sample2         A 
## sample3         A 
## sample4         B 
## sample5         B 
## sample6         B
```

我们使用这个`condition`因子变量来作为分组变量.
这样样本就按照condition来分成A,B两类

``` R
design(dds) <- ~condition
```

接下来让DESeq2进行模型的拟合,

``` R
dds <- DESeq(dds)
```

查看一下DESeq在这个模型中拟合得到的效应(系数)有哪些:

```R
resultsNames(dds)
```

得到如下输出:

``` bash
[1] "Intercept"        "condition_B_vs_A"
```

其中这个`condition_B_vs_A`是DESeq默认做出的contrast. 

使用默认参数的`results`函数取出`dds`结果.
得到的表达量fold change 指的是  \(\frac{condition\ B}{condition\ A}\)

效果等同于下面的代码:

```R
results(dds, contrast=c("condition", "B","A"))
```

上面的例子中, condition这个因子变量的levels中, A会排在B前面.
所以DESeq默认按照字母序使用A作为reference level,
结果中的fold change是基因在condition为B 样本中表达水平除以 condition A样本中的基因表达水平.

### 没有交互项双因素

我们这时候考虑模型中除了condition外,还有一个因素genotype.
展示不考虑这两者间的交互作用. 

那么模型的方程可以写成

```R
~ genotype + condition
```

如果说单因素的比较可以类比为蛋糕切两半,然后两半之间比较的话.
这种情况实际上和单因素一个样子, 差别只是condition和genotype分割方式不一样.两者依然各处理各的,不干扰.


### 加上一个交互项
