---
layout:     post
title:      "Q104 Maximum Depth of Binary Tree"
subtitle:   ""
date:       2020-09-07
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Medium
    - BFS
    - DFS
---

# [Maximum Depth of Binary Tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

## Question

> Given a binary tree, find its maximum depth.
>
> The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
>
> **Note:** A leaf is a node with no children.

> **Example 1:**
>
> Given binary tree [3,9,20,null,null,15,7],
>
> ```
>     3
>    / \
>   9  20
>     /  \
>    15   7
> ```
>
> return its depth = 3.

### Approach 1: 深度优先搜索

如果我们知道了左子树和右子树的最大深度 l 和 r，那么该二叉树的最大深度即为$$\max(l,r) + 1$$

而左子树和右子树的最大深度又可以以同样的方式进行计算。因此我们在计算当前二叉树的最大深度时，可以先递归计算出其左子树和右子树的最大深度，然后在 O(1)O(1) 时间内计算出当前二叉树的最大深度。递归在访问到空节点时退出。

```python
class Solution(object):
    def maxDepth(self, root):
        if root is None:
            return 0
        else:
            left_height = self.maxDepth(root.left)
            right_height = self.maxDepth(root.right)
            return max(left_height, right_height) + 1
```



### 复杂度分析

- 时间复杂度：$$O(n)$$，其中 n 为二叉树节点的个数。每个节点在递归中只被遍历一次。
- 空间复杂度：$$O(\textit{height})$$，其中 $$\textit{height}$$ 表示二叉树的高度。递归函数需要栈空间，而栈空间取决于递归的深度，因此空间复杂度等价于二叉树的高度。

### Approach 1: 广度优先搜索

我们也可以用「广度优先搜索」的方法来解决这道题目，但我们需要对其进行一些修改，此时我们广度优先搜索的队列里存放的是「当前层的所有节点」。每次拓展下一层的时候，不同于广度优先搜索的每次只从队列里拿出一个节点，我们需要将队列里的所有节点都拿出来进行拓展，这样能保证每次拓展完的时候队列里存放的是当前层的所有节点，即我们是一层一层地进行拓展，最后我们用一个变量 $$\textit{ans}$$ 来维护拓展的次数，该二叉树的最大深度即为 $$\textit{ans}$$。

```python
class Solution(object):
	def maxDepth2(self, root):
        if root is None:
            return 0
        queue = [(root, 1)]
        max_dep = 0
        while queue:
            node, depth = queue.pop()
            max_dep = max(max_dep, depth)
            if node.left:
                queue.append((node.left, depth +1))
            if node.right:
                queue.append((node.right, depth + 1))
        return max_dep
```

### 复杂度分析

时间复杂度：$$O(n)$$，其中 n 为二叉树的节点个数。与方法一同样的分析，每个节点只会被访问一次。
空间复杂度：此方法空间的消耗取决于队列存储的元素数量，其在最坏情况下会达到 $$O(n)$$。




- 

### 思考题

对于给定的排列 $$a_1, a_2, \cdots, a_n$$ ，你能求出 k 吗？

解答：
$$
k = \left( \sum_{i=1}^n \textit{order}(a_i) \cdot (n-i)! \right) + 1
$$
其中 $$\textit{order}(a_i)$$表示 $$a_{i+1}, \cdots, a_n$$中小于 $$a_i$$的元素个数。

### 反思

我在分析这个问题的时候只考虑到了第一项怎么确定，然后就用递归来求解问题，中间忽略了n和k的取值特例。分析不够完备，代码看着比较乱，可读性不强。今后分析的时候还是要把问题分析完整了再编代码，比如推出这道题的通项公式后再写代码。