---
bookCollapseSection: true
weight: 3
---

# 3.数组.链表.跳表
## Array时间复杂度
prepend O(1)
append  O(1)
lookup  O(1)
insert  O(n)
delete  O(n)

## 链表时间复杂度
prepend O(1)  
append  O(1)  
lookup  O(n)  
insert  O(1)  
delete  O(1)  

## skip List跳表
中心思想: 升维, 空间换时间  
加一级索引，二级索引  
查询的时间复杂度 O(logn), 删除和插入会重新生成索引
查询的空间复杂度是 O(n)

## 应用
* LRU Cache - Linked list： LRU 缓存机制  
https://leetcode-cn.com/problems/lru-cache/
* Redis - Skip List：跳跃表、为啥 Redis 使用跳表（Skip List）而不是使用 Red-Black？
https://redisbook.readthedocs.io/en/latest/internal-datastruct/skiplist.html  
https://www.zhihu.com/question/20202931  
