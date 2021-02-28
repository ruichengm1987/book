---
bookCollapseSection: true
weight: 15
---

# 15.红黑树和AVL树
## AVL树
* 1. 发明者是G.M.Adelson-Velsky和Evgenii Landis
* 2.Balance Factor (平衡因子) 
是它的左子树的高度减去它的右子树的高度(有时相反)。balance factor = {-1, 0, 1}
* 3.通过旋转操作来进行平衡(四种)
旋转操作:
   - 左旋
      子树形态:右右子树->左旋
      {{< img src="/architect/algorithm/15/1.png" title="" >}}
   - 右旋
      子树形态:左左子树->右旋
      {{< img src="/architect/algorithm/15/2.png" title="" >}}
   - 左右旋
      子树形态:左右子树->左右旋
      {{< img src="/architect/algorithm/15/3.png" title="" >}}
   - 右左旋
      子树形态:右左子树->右左旋
      {{< img src="/architect/algorithm/15/4.png" title="" >}}

总结:  
* 平衡二叉搜索树
* 每个结点存 balance factor = {-1, 0, 1}
* 四种旋转操作
不足: 结点需要存储额外信息、且调整次数频繁  

## 红黑树 Red-black Tree
红黑树是一种**近似平衡**的二叉搜索树(Binary Search Tree), 它能够确保任何一个节点的左右子树的高度差小于两倍。具体来说,红黑树是满足如下条件的
二叉搜索树:
* 每个节点要么是红色，要么是黑色
* 根结点是黑色
* 每个叶结点(NIL结点,空结点)是黑色的
* 不能有相邻的两个红色结点
* 从任一结点到其每个叶子的所有路径都博涵相同数目的黑色结点

### 关键性质
从根到叶子的最长的可能路径不多于最短的可能路径的两倍长。

## 对比
* AVL trees provide *faster lookups* than Red Black Trees because they are *more strictly balanced*
* Red Black Trees provide *faster insertion and removal* operations than AVL trees as fewer rotations are done due to 
relatively relaxed balancing
* AVL trees store balance *factors or heights* with each node, thus required storage for an integer per node whereas Red
Black Tree required only 1 bit of infomation per node.
* Red Black Trees are used in most of the *language libraries like map, multimap, multisetin C++* whereas AVL trees are used
in *databases* where faster retrievals are required.