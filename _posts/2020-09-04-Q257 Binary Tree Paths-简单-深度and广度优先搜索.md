---
layout:     post
title:      "Q257 Binary Tree Paths-简单-深度/广度优先搜索"
subtitle:   ""
date:       2020-09-04
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - DFS
    - BFS
---

# [Binary Tree Paths](https://leetcode-cn.com/problems/binary-tree-paths/)

## Question

> Given a binary tree, return all root-to-leaf paths.
>
> Note: A leaf is a node with no children.
>

> **Example 1:**
>
> Input:
> 
>     1
>    /   \
>   2     3
>    \
>  5
>  
>   Output: ["1->2->5", "1->3"]
>   
>   Explanation: All root-to-leaf paths are: 1->2->5, 1->3
> 

### Approach 1: 深度优先搜索

最直观的方法是使用深度优先搜索。在深度优先搜索遍历二叉树时，我们需要考虑当前的节点以及它的孩子节点。

如果当前节点不是叶子节点，则在当前的路径末尾添加该节点，并继续递归遍历该节点的每一个叶子节点。
如果当前节点是叶子节点，则在当前路径末尾添加该节点后我们就得到了一条从根节点到叶子节点的路径，将该路径加入到答案即可。
如此，当遍历完整棵二叉树以后我们就得到了所有从根节点到叶子节点的路径。当然，深度优先搜索也可以使用非递归的方式实现，这里不再赘述。

```python
class Solution:
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        def construct_paths(root, path):
            if root:
                path += str(root.val)
                if not root.left and not root.right:  # 当前节点是叶子节点
                    paths.append(path)  # 把路径加入到答案中
                else:
                    path += '->'  # 当前节点不是叶子节点，继续递归遍历
                    construct_paths(root.left, path)
                    construct_paths(root.right, path)

        paths = []
        construct_paths(root, '')
        return paths
```



### 复杂度分析

- 时间复杂度：$$O(N^2)$$，其中 N 表示节点数目。在深度优先搜索中每个节点会被访问一次且只会被访问一次，每一次会对 path 变量进行拷贝构造，时间代价为 O(N)，故时间复杂度为 $$ O(N^2)$$。

- 空间复杂度：$$O(N^2)$$，其中 N 表示节点数目。除答案数组外我们需要考虑递归调用的栈空间。在最坏情况下，当二叉树中每个节点只有一个孩子节点时，即整棵二叉树呈一个链状，此时递归的层数为 N，此时每一层的 path 变量的空间代价的总和为 $$O(\sum_{i = 1}^{N} i) = O(N^2)$$ 空间复杂度为 $$O(N^2)$$。最好情况下，当二叉树为平衡二叉树时，它的高度为 $$\log N$$，此时空间复杂度为 $$O((\log {N})^2)$$。

- **平衡二叉树**（Balanced Binary Tree）又被称为AVL树（有别于AVL算法），且具有以下性质：它是一 棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。

  

### Approach 2: 广度优先搜索
我们也可以用广度优先搜索来实现。我们维护一个队列，存储节点以及根到该节点的路径。一开始这个队列里只有根节点。在每一步迭代中，我们取出队列中的首节点，如果它是叶子节点，则将它对应的路径加入到答案中。如果它不是叶子节点，则将它的所有孩子节点加入到队列的末尾。当队列为空时广度优先搜索结束，我们即能得到答案。



```python
class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        paths = list()
        if not root:
            return paths

        node_queue = collections.deque([root])
        path_queue = collections.deque([str(root.val)])

        while node_queue:
            node = node_queue.popleft()
            path = path_queue.popleft()

            if not node.left and not node.right:
                paths.append(path)
            else:
                if node.left:
                    node_queue.append(node.left)
                    path_queue.append(path + '->' + str(node.left.val))
                
                if node.right:
                    node_queue.append(node.right)
                    path_queue.append(path + '->' + str(node.right.val))
        return paths

```

### 复杂度分析

- 时间复杂度：$$O(N^2)$$，其中 N 表示节点数目。分析同方法一。
- 空间复杂度：$$O(N^2)$$，其中 N 表示节点数目。在最坏情况下，队列中会存在 N 个节点，保存字符串的队列中每个节点的最大长度为 N，故空间复杂度为 $$O(N^2)$$



### 

