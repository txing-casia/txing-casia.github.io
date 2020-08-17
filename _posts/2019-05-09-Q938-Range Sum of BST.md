---
layout:     post
title:      "Q938 Range Sum of BST"
subtitle:   "binary search tree"
date:       2019-05-09
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Binary Search Tree
---

# [Range Sum of BST](https://leetcode.com/problems/range-sum-of-bst/)

## Question

> Given the `root` node of a binary search tree, return the sum of values of all nodes with value between `L` and `R` (inclusive).
>
> The binary search tree is guaranteed to have unique values.

> **Example 1:**
>
> Input: root = [10,5,15,3,7,null,18], L = 7, R = 15
> Output: 32

> **Example 2:**
>
> Input: root = [10,5,15,3,7,13,18,1,null,6], L = 6, R = 10
> Output: 23

> **Note:**
>
> 1. The number of nodes in the tree is at most `10000`.
> 2. The final answer is guaranteed to be less than `2^31`.

### Approach 1: Depth First Search

**Intuition and Algorithm**

We traverse the tree using a depth first search. If `node.val` falls outside the range `[L, R]`, (for example `node.val < L`), then we know that only the right branch could have nodes with value inside `[L, R]`.

We showcase two implementations - one using a recursive algorithm, and one using an iterative one.

**Recursive Implementation**

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def rangeSumBST(self, root, L, R):
        def dfs(node):
            if node:
                if L <= node.val <= R:
                    self.ans += node.val
                if L < node.val:
                    dfs(node.left)
                if node.val < R:
                    dfs(node.right)

        self.ans = 0
        dfs(root)
        return self.ans

```



**Iterative Implementation**

```python
class Solution(object):
    def rangeSumBST(self, root, L, R):
        ans = 0
        stack = [root]
        while stack:
            node = stack.pop()
            if node:
                if L <= node.val <= R:
                    ans += node.val
                if L < node.val:
                    stack.append(node.left)
                if node.val < R:
                    stack.append(node.right)
        return ans
```

