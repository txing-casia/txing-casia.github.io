---
layout:     post
title:      "Q766 Toeplitz Matrix"
subtitle:   "方法2提供了非常简洁的实现方式，faster than 98.04%的python代码"
date:       2019-04-30
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Array
---

# [Toeplitz Matrix](https://leetcode.com/problems/toeplitz-matrix/)

## Question

> A matrix is *Toeplitz* if every diagonal from top-left to bottom-right has the same element.
>
> Now given an `M x N` matrix, return `True` if and only if the matrix is *Toeplitz*.

> **Example 1**: 
>
> Input:
> matrix = [
>   [1,2,3,4],
>   [5,1,2,3],
>   [9,5,1,2]
> ]
> Output: True
> Explanation:
> In the above grid, the diagonals are:
> "[9]", "[5, 5]", "[1, 1, 1]", "[2, 2, 2]", "[3, 3]", "[4]".
> In each diagonal all elements are the same, so the answer is True.
>
> 

> **Example 2**: 
>
> Input:
> matrix = [
>   [1,2],
>   [2,2]
> ]
> Output: False
> Explanation:
> The diagonal "[1, 2]" has different elements.

> **Note:**
>
> - `matrix` will be a 2D array of integers.
> - `matrix` will have a number of rows and columns in range `[1, 20]`.
> - `matrix[i][j]` will be integers in range `[0, 99]`.

---

### Approach 1: Group by Category

**Intuition and Algorithm**

We ask what feature makes two coordinates `(r1, c1)` and `(r2, c2)` belong to the same diagonal?

It turns out two coordinates are on the same diagonal if and only if `r1 - c1 == r2 - c2`.

This leads to the following idea: remember the value of that diagonal as `groups[r-c]`. If we see a mismatch, the matrix is not Toeplitz; otherwise it is.

```python
class Solution:
    def isToeplitzMatrix(self, matrix) -> bool:
        groups = {}
        for r, row in enumerate(matrix):
            for c, val in enumerate(row):
                if (r - c) not in groups: # 行与列的差不在groups
                    groups[r - c] = val
                elif groups[r - c] != val:
                    return False
        return True
```

---

### Approach 2: Compare With Top-Left Neighbor

**Intuition and Algorithm**

For each diagonal with elements in order $a_1, a_2, a_3, \dots, a_k$, we can check $a_1 = a_2, a_2 = a_3, \dots, a_{k-1} = a_k$. The matrix is *Toeplitz* if and only if all of these conditions are true for all (top-left to bottom-right) diagonals.

Every element belongs to some diagonal, and it's previous element (if it exists) is it's top-left neighbor. Thus, for the square `(r, c)`, we only need to check `r == 0 OR c == 0 OR matrix[r-1][c-1] == matrix[r][c]`.

```python
class Solution:
    def isToeplitzMatrix(self, matrix) -> bool:
        return all(r == 0 or c == 0 or matrix[r-1][c-1] == val
                   for r, row in enumerate(matrix)
                   for c, val in enumerate(row))
    
    # all(),相当于or的含义，括号中有元素就返回True，无元素返回False
```

Runtime: 44 ms, faster than 98.04% of Python3 online submissions forToeplitz Matrix.

Memory Usage: 13.1 MB, less than 6.86% of Python3 online submissions for Toeplitz Matrix.