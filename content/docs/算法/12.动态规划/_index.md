---
bookCollapseSection: true
weight: 12
---

# 12.动态规划

## 感触
* 人肉递归低效、很累
* 找到最近最简单方法, 将其拆解成可重复解决的问题
* 数学归纳法思维, (抵制人肉递归的诱惑)

本质: 寻找重复性 -> 计算机指令集就那几个

## 动态规划
* wiki定义
https://en.wikipedia.org/wiki/Dynamic_programming
* "simplifying a complicated problem by breaking it down into simpler sub-problems"
(in a recursive manner) 将一个复杂的问题分解成多个子问题
* Divide & Conquer + Optimal substructure 分治+最优子结构

## 关键点
动态规划 和 递归或者分治 没有根本上的区别(关键看有无最优子结构)  
共性: 找到重复子问题  
差异性: 最优子结构、中途可以淘汰次解  

