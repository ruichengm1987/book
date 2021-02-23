---
bookCollapseSection: true
weight: 8
---

# 8.分治.回溯
本质上就是递归
最近重复性： 分治，回溯

分治的思想: 递归状态数的时候，对一个问题，它要化解成好几个子问题

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
    # process and generate the final result 
    result = process_result(subresult1, subresult2, subresult3, ...)
```