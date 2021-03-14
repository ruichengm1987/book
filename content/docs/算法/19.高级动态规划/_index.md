---
bookCollapseSection: true
weight: 19
---

# 19.高级动态规划
* "Simplifying a complicated problem by breaking it down into simpler sub-problems"
(in a recursive manner)

* Divide & Conquer + Optimal substructure  分治 + 最优子结构

* 顺推形式：动态递推

## Dp顺推模板
```$xslt
function DP():
    dp = [][] #二维情况
    for i = 0..M {
        for j = O..N {
            dp[i][j] = _function(dp[i][j]...)
        }
    }
    return dp[M][N]
```

## 关键点
动态规划和递归或者分治 没有根本上的区别(关键看有无最优的子结构)

拥有共性: 找到重复子问题

差异性: 最优子结构、中途可以淘汰次优解

