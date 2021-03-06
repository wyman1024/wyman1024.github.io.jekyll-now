---
layout: post_latex
title: 基数计数与hyperloglog算法
tags: ['redis']
published: false
---


<!--more-->

# 基数计数 cardinality counting

## 基数 cardinality

基数是指一个数据集中不同元素的个数。例下面的集合：

1,2,3,4,5,5,5,5

这个集合有8个元素，但是5出现了4次，因此不重复的元素为1,2,3,4,5，所以这个集合的基数是7。

如果一个集合是有限集，则其基数是一个自然数。

## 等势

如果两个集合具有相同的基数，我们说这两个集合等势。

## 大数据集的基数计数

要准确知道一个大数据集的基数计数，是非常困难的，首先要有一个地方（内存或硬盘），存放整个数据集不重复的元素，假设每个元素要占x字节，不重复元素数量为k，总共需要xk字节。

一般数据集还不止一个，可能是一个事件就对应一个数据集。

例如现在要统计一天内，某10个关键词，被多少个不重复用户（独立访客，Unique Visitor，简称UV）搜索过。因此有10个数据集。

再假设一个用户的UUID占16字节、每个关键词被100万以上个用户搜索，那么总共有：

10 * 16 * 1000 000 = 160 MB


可能和如今的内存、硬盘容量比起来不大，但如果现在需求变成统计几万个关键词、统计周期一个月，内存就撑不住了。

关键问题在于：统计能力和存储空间密切相关。存储空间越大，能统计的东西就越多。

存储也不一定能存在硬盘中，因为很可能统计是实时、高并发的，应在内存中进行统计。（不过也可能实现某种内存统计局部数据、定期硬盘合并的计数方式）

除了存储问题，还有查找问题，即如何确定一个用户是否已经被统计过了？

总之，基数计数一个和算法、数学、数据结构密切相关的问题。


# 传统精确基数计数

## 基于B树

## 基于bitmap


# 基于概率的尽量精确基数计数

## Linear Counting（1990年）





# 资料

http://blog.codinglabs.org/articles/algorithms-for-cardinality-estimation-part-i.html

http://blog.codinglabs.org/articles/algorithms-for-cardinality-estimation-part-ii.html

http://blog.codinglabs.org/articles/algorithms-for-cardinality-estimation-part-iii.html

http://blog.codinglabs.org/articles/algorithms-for-cardinality-estimation-part-iv.html


