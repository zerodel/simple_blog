---
title: HMMER
author: zerodel
date: '2021-09-11'
slug: hmmer_and_so_on
categories:
  - memo
tags:
  - bioinfo
---

# HMMER 的简要笔记

这里记录一下 [生信软件HMMER手册](https://www.ebi.ac.uk/Tools/hmmer/help)的笔记或者说备忘录.

## HMMER 是什么? 什么用处? 

HMMER 的功能类似于blast, 也是用来比对序列,它能够在给定的序列中找到特定的结构域.

它比普通的blast更精确,也更耗时.

类似blast ,它也有在线版本:[https://www.ebi.ac.uk/Tools/hmmer/](https://www.ebi.ac.uk/Tools/hmmer/)和本地运行版本.

接下来将介绍本地运行版本.

## 从安装到运行

安装HMMER 最简单的方法是使用 `conda` 来安装, 需要指定 `bioconda` 这个channel.  

安装好之后, HMMER套件中的一系列命令就可以使用了. 

其中 `hmmscan` 和 `hmmsearch` 是HMMER的核心功能.

如果结构域的信息已经记录成 hmm 格式,并用`hmmpress`建立了二进制库文件.
同时也有了序列文件(fasta/niprot/embl/genebank).

那么可以直接用这两个命令来查找序列中domain的分布情况, 并将运行结果记录成一个表格文件.

命令类似:
> hmmsearch globins4.hmm uniprot_sprot.fasta > globins4.out

其中globin4.hmm就是domain的hmm文件. 
uniprot_sprot.fasta 就是蛋白质序列文件.
而输出结果就是 globins4.out

hmmsearch 和hmmscan的核心功能实际上是一样的, 但查询的方式稍有差异.

按照 [比较hmmsearch/hmmscan](http://cryptogenomicon.org/hmmscan-vs-hmmsearch-speed-the-numerology.html) 的说法, 在很多序列上查询多个domain的时候, 使用`hmmsearch` 会更快. 

### 描述domain的hmm文件

正如它名字之中的HMM说明了它使用的数学模型是隐马尔柯夫模型,
它使用一个hmm的文本文件来描述某个结构域的信息. 

如果你有一个结构域的多重比对文件,那么你就可以使用 `hmmbuild`来建立这个结构域的hmm文件.

而这个多重比对文件, 可以从 pfam 数据库下载,
比如 pfam 数据库中domain页面的seed文件就可以.

多个hmm文件可以直接拼接起来使用, 拼接后的hmmsearch/hmmscan 结果就包含多个结构域.

在使用hmmscan/hmmsearch 之前,需要调用 `hmmpress` 来建立hmm模型的二进制库,以提升运行速度. 

## 运行结果如何解读?

### 基本概念

HMMER中使用了类似blast 的E值/bitscore 这些指标来说明同源性是否显著.

而一个蛋白质序列中可能存在多个domain, HMMER 将某个domain在序列上匹配到的区域称为 `envelope`, 这个区域会将这个domain包在里面. 边界可能会大一些. 

表现在HMMER查询结果的表格文件中, 就是你会同时看到序列的E值,和domain的E值. 

而研究者在确认序列同源性的时候,要考虑两个层次的结果: 序列层次 以及 domain 层次.

而且一个重复序列的重复部分和某个domain类似, 那么光是通过不停的重复, 整个序列也会在统计学上显得显著. 

所以得到比对结果后,要同时考虑这两个层次的结果才能确认序列的真实情况.

### 结果文件的解读

HMMER的不同命令得到的结果文件中,列的含义往往是统一的, 所以这里的介绍不一定一一对应某个特定的命令结果.

#### hmmsearch 

hmmsearch 运行结果是一个文本文件, 文件头部为每行开头为#的注释, 之后就是结果的数据表格,最后可能会有一个运行的总结.

注释记录了本次结果运行的参数. 而数据表格类似于

![](https://raw.githubusercontent.com/zerodel/img_host/main/img/hmmsearch%E7%BB%93%E6%9E%9C%E8%A1%A8%E6%A0%BC202109111731776.png)

可以看到表头可以分成三部分 :
`full sequence` : 序列层次的统计学显著性结果
`best 1 domain` : 最显著的domain的统计学结果
`# dom` :  序列上关于domain的一些说明信息.

而序列层次和domain层次的统计学结果都包含三列:  `E-value`, `score` `bias`. 

其中 `E-value`的含义和blast中的Evalue类似, 都是在描述如果搜索的数据库中的序列如果都是随机的, 能产生多少条和当前结果一样显著的匹配(hits).

`score` 就是bit score, 它描述了当前匹配的显著性, Evalue也是从它的基础上计算得来. 而且bit score 不依赖序列数据库的大小. 比如之前命令中的uniprot_sprot.fasta中的序列个数.

`bias` 是对score的调整. 一般来说, 当 bias的数值接近与score的数量级的时候, 这个匹配的结果的可信度就下降了.


`#dom` 部分的exp和N 分别表示了序列中预测的domain数量,和最终确定的domain数量. 两者差异很大,说明序列是一个重复序列.

##### 序列的E值和domain的E值 如何结合起来判断序列

如果两个E值都远远小于1, 那么序列可以认为是显著同源.

如果序列E值显著, 而domain的E值不那么显著, 那么序列可能存在多个domain,或者序列是重复序列.

而如果序列是重复序列, 那么 #dom 部分 的exp 和N 两列的差异也会比较大.

#### hmmscan

如果使用如下的命令运行 hmmscan

> hmmscan --domtblout ./piwi_should_have.txt ./piwi_seed.hmm prot.fa

其中 piwi_seed.hmm 是domain的信息, 而prot.fa 是蛋白质序列.

那么得到的结果可能是这样的:

![](https://raw.githubusercontent.com/zerodel/img_host/main/img/hmmscan_domtblout.png)

运行结果中的表格各列也从左到右分成了几个部分:

最左边分别是当前匹配涉及到的 domain和序列的基本信息. 

然后红框中是序列层次的统计量, 含义和上文中的一致. 

接下来就是当前匹配中domain的统计量.  注意这里存在两个E值, c-Evalue , i-Evalue

其中 i-Evalue(independent Evalue)的含义和之前的domain 的E值一致. 

而 c-Evalue(conditional Evalue) 表示的是:如果当前序列显著,那么这个domain有多显著.这个参数可能会被废弃. 

然后是当前匹配在domain和序列中的起止坐标(from...to...). 这里面也分成三类. 
hmm coord 表示这个匹配在domain上的起止. 
ali coord 表示这个匹配在序列上的起止. 
env coord 表示这个匹配的`envelope` 的起止. 

而手册上建议使用 env coord 来表示匹配在序列上的范围.

再往后的 acc 表示了匹配片段上每个残基的accuracy期望值.

最后是domain的说明,这里缺失了. 


## 小结

HMMER 和blast 很相似, 功能类似,运行过程也类似.

但HMMER需要从多重比对文件开始建立domain的数据库,然后依照这个数据库来搜索蛋白质序列中同源的匹配.
并由于蛋白质中可能存在多个domain, 所以在结果的呈现上也和blast有较大区别.
