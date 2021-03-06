---
layout: post
title:  "无人机5g与快递"
date:   2020-11-19
categories: [thoughts]
---

# 无人机5g与快递

## 起因
首先是听说申通中通都开始开始有快递员罢工了. 看了一下可能是营收状况恶化的结果: [快递在一月营收锐减](https://www.qianzhan.com/analyst/detail/220/200304-e2ee4478.html)
另外阿里自己的菜鸟驿站的体验也不好, "打工人" 往往需要走好远才能去取回快递.

### 无人机用在快递上的挑战

搜索了一下无人机快递的一些情况,比如[无人机快递“从天而降” 不仅要快更要“稳”](http://www.xinhuanet.com/tech/2020-04/29/c_1125920505.htm)这个文章中提到无人机快递的缺陷

#### 续航问题

```从技术层面看，无人机物流配送除了“快”还需要“稳”。续航问题一直是无人机的痛点所在，而将无人机运用到物流配送领域，续航能力更是重中之重。动力电池的比能量（单位质量/体积的器件可提供的能量）是一个重要指标，每增加一公斤载重，无人机航时会成倍缩短。此外，在不同飞行环境下，不同用途的无人机对传感器的配置要求也不同。通过加装避障功能模块，整合迫近传感器与飞控系统，可以避免无人机坠毁。```

#### 导航问题

```在导航方面，目前无人机载导航系统主要分非自主（GPS等）和自主（惯性制导）两种，但分别有易受干扰和误差积累增大的缺点，多种导航技术（惯性+多传感器+GPS+光电导航系统），采用滤波器运行方式将是未来发展的方向。```

#### 航线审批

```“从政策来看，无人机的应用避免不了空域管理的问题。”白子龙认为，在相当长的时间内，无人机配送还难以实现大规模的商业化运作。“目前开辟航线必须经过严格的日审批，也就是除航线审批外，起飞的前一天需要获得许可，然后起飞的当天还必须再次确认审批。从实践情况来看，城市地区空域管理更为严格，空域和飞行计划审批程序也存在着较大优化空间。”```

法规要求无人机不得离开人的视线,以及不能在人头顶上飞行

```此外，无人机在空中飞行也需要进行交通管理。比如无人机送货离不开超视距飞行和在人群上空飞行，目前国外在立法上普遍禁止无人机超视距飞行和在人群上空飞行，除非能够得到监管机构的豁免。因此，无人机物流配送法律法规、运营标准、管理制度等方面尚需进一步探索。```


## 无人机 + 5G , 如何?


一个念头浮现出来, 无人机快递 和5g 联合起来, 有没有搞头?
让信息网节点和物流网节点重合, 怎么样?

### 已有的5G无人机

这个思路已经有人提到了, [5g无人机](http://finance.sina.com.cn/stock/stockzmt/2020-01-05/doc-iihnzhha0517080.shtml) 或者说网联无人机.
华为也有做这个的[5g无人机用于救援](https://www.sohu.com/a/407211755_100126234)

这里面的应用,往往是在城市以外,而无人机现在看到的文章都在强调它们能够应对任何地点到任何地点的飞行.

但5g基站最多的地方是什么?大城市啊, 快递需求最杂最多的地方是哪儿? 大城市
大城市的各个地点都是固定的. 不需要那么灵活.

### 如果我们的重点是基站, 而不是无人机的话 ......

我的基本想法是无人机不需要那么自主. 让无人机成为5G基站的附属.

设施上说,就是5G基站加装光电系统,为加装5g终端的无人机提供导航,以及为无人机充电
然后搭配自动快递货柜.

通过5G基站的充电器在路线之中给无人机充电,应该可以提高无人机的续航.

飞行路线固定了, 导航要求就降低了. 5g基站的信号用来你导航, 肯定比手持天线强.
而且基站间路程固定, 误差什么的也好办.

另一方面基站导航的话, 对无人机的控制权就可以归到一点,而不是每个买了大疆的年轻人手上.
这个可以避免"黑飞". 毕竟这种情况下,无人机就是渡轮.
利用5g的基站来给无人机导航, 将无人机的飞行范围限定在基站之间连线上. 这样,航线审批应该能降低难度.

#### 基站所需的准备

基站上加装一些光电头, 可以随时定位并"凝视"无人机的来往. 这样也可以做到随时"人在回路", 人可以时刻通过5G基站摄像视频来监控无人机的运行. 不算"超视距"了吧. 

对于5G基站, 本身5G需要更多的基站,来完成覆盖. 但是从4G的推广过程来看, 得让人有些实惠, 才好推广. 不然的话就开始"避邻"了.
5G搭配无人机的快递服务,5G信号覆盖到哪里, 无人机就能送外卖到哪里,给人们一个接受的理由.

加一套光电头以及相关的给无人机导航的系统, 无人机只要从一个基站飞到另一个基站. 这个任务计算量应该不算大.
功耗的提升,我觉得应该不算大.

给无人机充电, 相比基站自己的耗电, 我想也算不上多大的功耗.
但这样却可以降低无人机自己的续航需求. 提高冗余度,

#### 对无人机的要求可以降低

对于无人机来说, 导航要求,续航要求由地面基站承担那么无人机成本可以降低, 数量可以增加,那么运送就可以更方便.

货柜和基站不一定要在一个地方. 因为基站要考虑信号覆盖, 而货柜要考虑人能方便的取货.
但货柜应该放在能从基站直接看到的地方,这样基站的光电头可以直接凝视送货的无人机,指导它降落到货柜.

如果我们放宽飞机的一点要求, 就是不一定只是在基站和货柜之间飞行, 而是要求在最后一部分在5g基站的"注视"下可以自由一点的选择落点. 
那么就可以送快递, 买药什么的了.....

#### 快递的货柜如何与基站整合

对于货柜, 丰巢看上去就是普通柜子集成电子锁,存取都是开同一个柜门. 柜子的小格是半封闭的,只有门一个开口.

我的想法是如果货柜需要自动化的话, 那么类似蜂巢. 每个柜子也有个电子锁.
只是柜子里的每个小格后端不再封闭, 至少有个开启的活门.
这样的话, 后面搞一个竖着的xy平台, 来分发快递到各个小格.xy平台的z方向,用来放置可以固定快递或者无人机的托盘.

如果从成本的角度来说.
货物的分发可以使用另一种解决方案, 就是利用现有的物业系统, 也就是货物存放在物业的地点,让他们来管理快递的分发.

## 小结

无人机发展起来是因为它的"自由",但这种"自由"也是限制它得以规模化应用,毕竟被规模化的事物都是受限的.

说到快递问题, 如果依赖无人机来自己完成快递,
那么从技术层面上说, 我们就需要无人机自己的机体来解决导航/续航问题,同时对它们的控制也会是个问题.

如果以5G基站为中心, 为无人机提供充电和导航,那么以上的问题都可以得到缓解,
这对5G的推广和必将到来的快递员荒,应该也有所帮助.
