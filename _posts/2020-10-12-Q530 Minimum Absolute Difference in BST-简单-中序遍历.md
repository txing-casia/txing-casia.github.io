---
layout:     post
title:      "Q530 Minimum Absolute Difference in BST-简单-中序遍历"
subtitle:   ""
date:       2020-10-12
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
---

# [Minimum Absolute Difference in BST](https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/)

## Question

> Given a binary search tree with non-negative values, find the minimum absolute difference between values of any two nodes.
>
> **Note:**
>
> There are at least two nodes in this BST.
> This question is the same as 783: https://leetcode.com/problems/minimum-distance-between-bst-nodes/

> **Example 1:**
>
> Input:
>
>    1
>     \
>      3
>     /
>    2
>
> Output:
> 1
>
> Explanation:
> The minimum absolute difference is 1, which is the difference between 2 and 1 (or between 2 and 3).

### Approach 1: 中序遍历

考虑对升序数组 a 求任意两个元素之差的绝对值的最小值，答案一定为相邻两个元素之差的最小值，即

$$\textit{ans}=\min_{i=0}^{n-2}\left\{a[i+1]-a[i]\right\}$$

其中 n 为数组 a 的长度。其他任意间隔距离大于等于 2 的下标对 (i,j) 的元素之差一定大于下标对 (i,i+1) 的元素之差，故不需要再被考虑。

回到本题，本题要求二叉搜索树任意两节点差的绝对值的最小值，而我们知道二叉搜索树有个性质为**二叉搜索树中序遍历得到的值序列是递增有序的**，因此我们只要得到中序遍历后的值序列即能用上文提及的方法来解决。

朴素的方法是经过一次中序遍历将值保存在一个数组中再进行遍历求解，我们也可以在中序遍历的过程中用 $$\textit{pre}$$ 变量保存前驱节点的值，这样即能边遍历边更新答案，不再需要显式创建数组来保存，需要注意的是 $$\textit{pre}$$ 的初始值需要设置成任意负数标记开头，下文代码中设置为 -1。

二叉树的中序遍历有多种方式，包括递归、栈、Morris 遍历等，读者可选择自己最擅长的来实现。下文代码提供最普遍的递归方法来实现，其他遍历方法的介绍可以详细看[「94. 二叉树的中序遍历」](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)的题解，这里不再赘述。

```python
class Solution(object):
    def getMinimumDifference(self, root: TreeNode) -> int:
        def dfs(root):
            if not root:
                return
            dfs(root.left)
            nums.append(root.val)
            dfs(root.right)

        nums = []
        dfs(root)
        res = float('inf')
        for i in range(1, len(nums)):
            res = min(res, nums[i] - nums[i - 1])
        return res
```

**复杂度分析**

- 时间复杂度：O(n)，其中 n 为二叉搜索树节点的个数。每个节点在中序遍历中都会被访问一次且只会被访问一次，因此总时间复杂度为 O(n)。
- 空间复杂度：O(n)。递归函数的空间复杂度取决于递归的栈深度，而栈深度在二叉搜索树为一条链的情况下会达到 O(n) 级别。

### Approach 2: 中序遍历（空间优化）

保留前驱结点，滚动式更新结果

```python
class Solution:
    def getMinimumDifference(self, root: TreeNode) -> int:
        def dfs(root):
            nonlocal res, pre
            if not root:
                return 
            dfs(root.left)
            if pre != -1:
                res = min(res, root.val-pre)
            pre = root.val
            dfs(root.right)
        
        pre = -1
        res = float('inf')
        dfs(root)
        return res
```

