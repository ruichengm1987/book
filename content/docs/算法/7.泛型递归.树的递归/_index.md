---
bookCollapseSection: true
weight: 7
---

# 7.泛型递归.树的递归
盗梦空间  
* 向下进入到不同梦境中;向上又回到原来一层  
* 通过声音同步回到上一层
* 每一层的环境和周围的人都是一份拷贝、主角等几个人穿越不同层级的梦境(发生和携带变化)

## 代码模板
```$xslt
def recursion(level, param1, param2, ...):
    # recursion terminator 递归终结条件
    if level > MAX_LEVEL:
        process_result
        return
       
    # process logic in current level 处理当前层逻辑
    process(level, data...)

    # dril down 下探到下一层
    self.recursion(level+1, p1, ...)

    # reverse the current level status if needed 清理当前层
```
    
## 思维要点
* 1. 不要人肉进行递归(最大误区)
* 2. 找到最近最简方法, 将其拆解成可重复解决的问题(重复子问题)
* 3. 数学归纳法思维


    