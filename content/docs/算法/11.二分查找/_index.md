---
bookCollapseSection: true
weight: 11
---

# 11.二分查找
## 二分查找的前提
1. 目标函数单调性(单调递增或者递减)
2. 存在上下界（bounded）
3. 能够通过索引访问 (index accessible)

## 代码模板
```
left, right = 0, len(array) - 1
while left <= right:
    mid = (left + right)/ 2
    if array[mid] == target:
        # find the target!!
        break or return result
    elif array[mid] < target:
        left = mid + 1
    else:
        right = mid - 1
```
