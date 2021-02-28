---
bookCollapseSection: true
weight: 17
---

# 17.布隆过滤器和LRU缓存
## 布隆过滤器
### bloom Filter vs Hash Table
{{< img src="/architect/algorithm/17/1.png" title="" >}}
### 布隆过滤器示例图
{{< img src="/architect/algorithm/17/2.png" title="" >}}
{{< img src="/architect/algorithm/17/3.png" title="" >}}
### 案例
* 比特币网络
* 分布式系统(Map-Reduce) - Hadoop、search engine
* Redis 缓存
* 垃圾邮件、评论等的过滤

### 代码示例
```$xslt
todo::补充golang的布朗过滤器代码

```

## LRU Cache
* 两个要素: 大小、替换策略 
* Hash Table + Double LinkedList
* O(1)查询
* O(1)修改、更新

### 替换策略
* LFU least frequently used
* LRU least recently used
[替换算法总览](https://en.wikipedia.org/wiki/Cache_replacement_policies)

### 代码示例
```$xslt
todo::补充golang Lrucache
```
