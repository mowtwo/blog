---
title: 【动态规划】最简单的一维dp
date: 2022/01/06
description: 开始学习算法了，简单理解一维数组下的动态规划写法
tag: 动态规划,DP,算法
author: Mowtwo
---

# 【动态规划】最简单的一维dp
最近开始学习算法，算法一直都是我薄弱的部分。  
选择了LeetCode作为自己起步的平台，简单的刷了几题后，发现使用JavaScript刷算法是简单很多，一些不涉及到数学思维的算法题可以很快的解决。  
不过刷到动态规划题目时，大都让我一脸懵逼，回想自己第一次做0/1背包时候的窘状，我决定打破困境，从头学起。

## 为什么需要动态规划
动态规划其实是一种很暴力的解题方式，当然，我刷算法题不只是为了刷题目，我是想要结合项目进行应用，所以不能像大部分人一样，简单的记录下解法通项式就结束，我的思考方式也不允许我做这种事情。所以我必须思考动态规划的实际应用场景。  
首先明确动态规划的本质就是为了查询或者搜索而存在的，一般在大数据集或者带有固定路径的索引集合里进行使用。而在做这些搜索操作时，往往会出现大量的重复性结果集，如果每次都要重新求解，就会浪费大量时间。所以动态规划的目的就是利用“历史记录”的存在，将已有的结果集重复利用。  
而动态规划的本质就是如何构建历史记录，并且能在求解下一个结果时，引用到历史记录，而想要引用历史记录，必须构建结果集之间的通项式关系，这就是动态规划的难点所在。

## 如何构建通项式
构建通项式其实就跟高中数学里的  

## 未完待续