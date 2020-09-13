---
layout:     post
title:      "Q637 Average of Levels in Binary Tree-简单-深度/广度优先搜索"
subtitle:   ""
date:       2020-09-12
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - DFS
    - BFS
---

# [Average of Levels in Binary Tree](https://leetcode-cn.com/problems/average-of-levels-in-binary-tree/)

## Question

> Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.
> Example 1:
> Input:
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> Output: [3, 14.5, 11]
> Explanation: The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].
>
> **Note:**
>
> The range of node's value is in the range of 32-bit signed integer.

## Approach 1: 深度优先搜索
使用深度优先搜索计算二叉树的层平均值，需要维护两个数组，$$\textit{counts}$$ 用于存储二叉树的每一层的节点数，$$\textit{sums}$$ 用于存储二叉树的每一层的节点值之和。搜索过程中需要记录当前节点所在层，如果访问到的节点在第 i 层，则将 $$\textit{counts}[i]$$ 的值加 1，并将该节点的值加到 $$\textit{sums}[i]$$。

遍历结束之后，第 i 层的平均值即为 $$\textit{sums}[i] / \textit{counts}[i]$$。

```python
class Solution(object):
    def averageOfLevels_dfs(self, root):
        def dfs(root:TreeNode, level):
            if not root:
                return
            if level < len(totals):
                totals[level] += root.val
                counts[level] += 1
            else:
                totals.append(root.val)
                counts.append(1)
            dfs(root.left, level + 1)
            dfs(root.right, level + 1)

        counts = list()
        totals = list()
        dfs(root, 0)
        return [total / count for total, count in zip(totals, counts)]
```

### 复杂度分析

- 时间复杂度：$$O(n)$$，其中 n 是二叉树中的节点个数。
  深度优先搜索需要对每个节点访问一次，对于每个节点，维护两个数组的时间复杂度都是 $$O(1)$$，因此深度优先搜索的时间复杂度是 $$O(n)$$。遍历结束之后计算每层的平均值的时间复杂度是 O(h)O(h)，其中 hh 是二叉树的高度，任何情况下都满足 $$h \le n$$。因此总时间复杂度是 $$O(n)$$。

- 空间复杂度：$$O(n)$$，其中 n 是二叉树中的节点个数。空间复杂度取决于两个数组的大小和递归调用的层数，两个数组的大小都等于二叉树的高度，递归调用的层数不会超过二叉树的高度，最坏情况下，二叉树的高度等于节点个数。

## Approach 2: 广度优先搜索

也可以使用广度优先搜索计算二叉树的层平均值。从根节点开始搜索，每一轮遍历同一层的全部节点，计算该层的节点数以及该层的节点值之和，然后计算该层的平均值。

如何确保每一轮遍历的是同一层的全部节点呢？我们可以借鉴层次遍历的做法，广度优先搜索使用队列存储待访问节点，只要确保在每一轮遍历时，队列中的节点是同一层的全部节点即可。具体做法如下：

- 初始时，将根节点加入队列；

- 每一轮遍历时，将队列中的节点全部取出，计算这些节点的数量以及它们的节点值之和，并计算这些节点的平均值，然后将这些节点的全部非空子节点加入队列，重复上述操作直到队列为空，遍历结束。

由于初始时队列中只有根节点，满足队列中的节点是同一层的全部节点，每一轮遍历时都会将队列中的当前层节点全部取出，并将下一层的全部节点加入队列，因此可以确保每一轮遍历的是同一层的全部节点。

具体实现方面，可以在每一轮遍历之前获得队列中的节点数量 $$\textit{size}$$，遍历时只遍历 $$size$$ 个节点，即可满足每一轮遍历的是同一层的全部节点。

```python
class Solution(object):
    def averageOfLevels_dfs(self, root):        
        averages = list()
        queue = collections.deque([root])
        while queue:
            total = 0
            size = len(queue)
            for _ in range(size):
                node = queue.popleft()
                total += node.val
                left, right = node.left, node.right
                if left:
                    queue.append(left)
                if right:
                    queue.append(right)
            averages.append(total / size)
        return averages
```

### 复杂度分析

- 时间复杂度：$$O(n)$$，其中 n 是二叉树中的节点个数。广度优先搜索需要对每个节点访问一次，时间复杂度是 $$O(n)$$。
  需要对二叉树的每一层计算平均值，时间复杂度是 $$O(h)$$，其中 h 是二叉树的高度，任何情况下都满足 $$h \le n$$。因此总时间复杂度是 $$O(n)$$。

- 空间复杂度：$$O(n)$$，其中 n 是二叉树中的节点个数。空间复杂度取决于队列开销，队列中的节点个数不会超过 n。

