---
bookCollapseSection: true
weight: 9
---

# 9.深度优先搜索和广度优先搜索
* 每个节点都要访问一次  
* 每个节点仅仅要访问一次 
* 对于节点的访问顺序不限
   - 深度优先: depth first search
   - 广度优先: breadth first search
   - 优先级优先搜索:
   
## 深度优先
示例代码:
```$xslt
def dfs(node):
    if node in visited:
        # already visited
        return
    visited.add(node)

    # process current node
    # ... # logic here
    dfs(node.left)
    dfs(node.right)


def dfs(node, visted):
    if node in visited: 
        # already visited
        return

    visted.add(node)
    # process current node here
    ...

    for next_node in node.children():
        if not next_node in visited:
            dfs(next node, visted)
```
```$xslt
# 非递归写法
def DFs(self, tree):
    if tree.root is None:
        return []
    visited, stack = [], [tree.root]
    
    while stack:
        node = stack.pop()
        visited.add(node)
        
        process(node)
        nodes = generate_related_nodes(node)
        stack.push(nodes)
    
    # other processing work
    ...
```
## 广度优先
```$xslt
def BFS(graph, start, end):
    queue = []
    queue.append([start])
    visited.add(start)

    while queue:
        node = queue.pop()
        visited.add(node)
        
        process(node)
        nodes = generate_related_nodes(node)
        queue.push(nodes)
    # other processing work
    ...
```
