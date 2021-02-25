---
bookCollapseSection: true
weight: 8
---

# 8.分治.回溯
本质上就是递归
最近重复性： 分治，回溯

分治的思想: 递归状态数的时候，对一个问题，它要化解成好几个子问题

## 分治
不管是分治或者是回溯或者是其他的叉叉叉的办法的话,其实最后本质上就是找重复性以及分解问题和最后组合每个子问题的结果

分治代码模板
```$xslt
def devide_conquer(problem, param1, param2, ...):
    # recursion terminator 问题解决判断
    if problem is None:
        print_result
        return
    #prepare data 处理当前层逻辑
    data = prepare_data(problem)
    subproblems = split_problem(problem, data)
    # conquer subproblems 下探到下一层
    subresult1 = self.devide_conquer(subproblems[0], p1, ...)
    subresult2 = self.devide_conquer(subproblems[1], p1, ...)
    subresult3 = self.devide_conquer(subproblems[2], p1, ...)
    ...
    # process and generate the final result  处理并生成最终结果
    result = process_result(subresult1, subresult2, subresult3, ...)
```

## 回溯
回溯法是采用试错的思想, 它尝试分布的去解决一个问题。在分布解决问题的过程中，当它通过
尝试发现现有的分布答案不能得到有效的正确的解答的时候, 它将取消上一步甚至是上几步的计算，
在通过其它的可能的分布解答再次尝试寻找问题的答案。

回溯法通常用最简单的递归方法来实现，在反复重复上述的步骤后可能出现靓装情况:  
* 找到一个可能存在的正确的答案
* 在尝试了所有可能的分布方法后宣告该问题没有答案

在最坏的情况下, 回溯法会导致一次复杂度为指数时间的计算。

代码模板和范递归相似

