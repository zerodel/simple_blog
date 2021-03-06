---
layout: post
title:  "GO数据库与GO分析的简单概念"
date:   2020-11-02
categories: [memo]
---

# GO数据库的基本概念 #

<!-- go是什么 -->
## GO 是什么? 

GO 是基因本体论(gene ontology) 的缩写. 本体论, 又称为"存在论", 和"方法论""认识论"是哲学里面的词汇. 它指的是探索本源的理论. 
我们这里不讨论那么多,只需要将GO理解为对基因产物功能的阐述. 

<!-- 为什么有go -->
从生物信息学实际操作的角度, 我们一般是得到样本中基因表达水平之后才会接触到GO.
这个时候,数据中基因往往只是一堆编号. 自然人是无法直接理解那些数字是字母的组合的,需要在后续的分析中将这些编号和细胞中基因的功能联系起来.

GO数据库就是将基因的信息整合并标准化,从而将基因与其功能联系起来. 

与KEGG数据库的区别在于, GO数据库着重于对基因产物本身的功能描述, 没有使用通路(pathway)来组织基因的信息.所以在实际分析过程中,常常是GO/KEGG同时使用.

<!-- go term的三个方面 -->

## GO的三个方面

GO从三个方面来描述一个基因产物: 细胞组分（cellular component, CC）、分子功能（molecular function, MF）、生物过程（biological process, BP）

细胞组分,描述了基因产物在细胞中的物理位置, 比如在叶绿体中,细胞膜中.

分子功能, 描述了基因产物这个分子相关的活动(activity), 比如催化,

生物过程, 描述了基因产物参与了哪种生物多分支协同过程, 比如代谢,免疫等等.

这些功能的标准化语言描述被称为"term", 并在GO数据库中关联着唯一的GO ID. 

<!-- term之间的DAG -->
## term 之间关系怎么说明?

在GO数据库中, GO term之间的关系被描述成DAG(有向无环图 directed acyclic graph), 

这个图中, 节点(node)是GO term, 边(edge)就是节点之间的关系. 
主要有下面三种关系

1. is-a
可以近似的理解为 "a是一种b", 
2. part-of
可以理解为 "a是b的一部分"

3. regulates
可以理解为"a对b有影响"

这三种关系可以叠加,也就是可以从图中一个节点出发,到达另一个节点. 
叠加的运算关系这里不再赘述. 

<!-- 何时使用go,以及背后的原理 -->
# GO分析

## 什么时候使用GO分析?

组学实验的结果往往规模巨大, 涉及到上万种基因产物的表达的差异. 
直接考虑这些基因层次的差异与表型之间的关联,就必须要考虑多重检验的问题. 

研究者们需要将这些数据"降维", 试图将分析对象从数万个基因减少到数量更少的"集团".

所以有了gene set 为基础的GSEA, 关注基因间共表达的WGCNA,pathway 为基础的KEGG分析. GO分析也是其中一种, 它从GO term 出发, 尝试探索哪些term对应基因存在更显著的差异. 

<!-- 如何使用GO -->

## 常见的GO分析是怎么做的

GO分析从差异表达分析得到的DEG(differential expressed gene)入手,通过超几何分布检验来判断这些DEG在哪些term中存在"富集". 

### 富集与超几何检验 

超几何检验常用来描述采样是否偏好某个子集.
它基于超几何分布这种离散分布, 比如说, 一个口袋中有N个球,其中M个球是黑球, 那么取出n个球, 其中有k个黑球的概率就服从超几何分布. 

\(p = \frac{C_M^k * C_{N-M}^{n-k}}{C_N^n}\)

而在GO分析的语境中, 作为总体的基因被称为背景基因(background gene), 而总体中被关心的子集就是前景基因(forground gene). 

比如人类基因组一共有 N 个基因, 其中某个GO term foo关联了M个基因, 实验得到的DEG一个有n个, 其中k个基因与该GO term 有关, 那么就可以再次使用上面的公式来描述了. 

GO分析的检验中,零假设是DEG中属于GOterm相关的基因集合的基因与不属于这个GOterm的基因在分布上没有差异.

### 实际操作

GO分析可以使用线上工具完成, 比如DAVID网站. 
也可以在R中利用bioconductor中的topGO等包来完成,
目前值得推荐的是clusterprofiler包, 它对多种富集分析做了一个集成,并提供了一系列绘图与工具函数. 

## 缺陷 与 GSEA

GO分析使用DEG作为输入, 但是差异表达分析结果是要使用一个p值的阈值来确定某个基因是否属于DEG的, 这个阈值的选择就比较主观. 不同的阈值得到的DEG不同,GO分析的结果就不一样.

很多时候,GO分析并没法得到有显著意义的结果.

对于这种情况的改进方法是采用GSEA来规避这种主观的阈值选择. GSEA更类似一种非参的检验方法,可以利用所有基因的表达变化信息,而不只是采用DEG的那部分. 


# 小结

GO数据库提供了基因产物功能的标准化描述,并给出基因产物功能之间的关联. 
GO富集分析通过超几何检验来判断差异表达基因与GO数据库中基因功能是否存在关联. 

研究者常常同时使用GO富集分析与KEGG通路分析来理解样本中的基因功能. 
而GO分析依赖差异表达的结果,容易受到差异表达阈值的干扰,此时,可以选择GSEA. 
