---
layout:     post
title:      "Q226 Invert Binary Tree-简单-递归"
subtitle:   ""
date:       2020-09-16
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - Recursion
---

# [Invert Binary Tree](https://leetcode-cn.com/problems/invert-binary-tree/)

## Question

> Invert a binary tree.
>
> Trivia:
> This problem was inspired by this original tweet by Max Howell:
>
> Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.
>

> **Example 1:**
>
> Input: 
>
> ```
>         4
>       /   \
>     2     7
>   / \   / \
> 1   3 6   9
> ```
>
> Output:
>
> ```
>         4
>       /   \
>     7     2
>   / \   / \
> 9   6 3   1
> ```
>
> 

### Approach 1: 递归
这是一道很经典的二叉树问题。显然，我们从根节点开始，递归地对树进行遍历，并从叶子结点先开始翻转。如果当前遍历到的节点 $$\textit{root}$$ 的左右两棵子树都已经翻转，那么我们只需要交换两棵子树的位置，即可完成以 $$\textit{root}$$ 为根节点的整棵子树的翻转。

```python
class TreeNode(object):
    def __init__(self,data = 0,left = None,right = None):
        self.val = data
        self.left = left
        self.right = right
###############################################################

class Solution(object):
    def invertTree(self, root) -> TreeNode:
        if not root:
            return root
        # 找到叶子节点开始反转
        left = self.invertTree(root.left)
        right = self.invertTree(root.right)
        root.left, root.right = right, left
        return root

 
def main():
    candies = TreeNode([2, 3, 5, 1, 3])
    extraCandies = 3
    solution=Solution().invertTree(candies)
    print(solution)


if __name__ == "__main__":
    main()

```

### 复杂度分析

- 时间复杂度：O(N)，其中 N 为二叉树节点的数目。我们会遍历二叉树中的每一个节点，对每个节点而言，我们在常数时间内交换其两棵子树。

- 空间复杂度：O(N)。使用的空间由递归栈的深度决定，它等于当前节点在二叉树中的高度。在平均情况下，二叉树的高度与节点个数为对数关系，即 $$O(\log N)$$。而在最坏情况下，树形成链状，空间复杂度为 O(N)。




