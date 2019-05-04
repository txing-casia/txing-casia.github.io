---
layout:     post
title:      "Q867 Transpose Matrix"
subtitle:   ""
date:       2019-05-3
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
---

# [Transpose Matrix](https://leetcode.com/problems/transpose-matrix/)

## Question

> Given a matrix `A`, return the transpose of `A`.
>
> The transpose of a matrix is the matrix flipped over it's main diagonal, switching the row and column indices of the matrix.

> **Example 1**: 
>
> Input: [[1,2,3],[4,5,6],[7,8,9]]
> Output: [[1,4,7],[2,5,8],[3,6,9]]
>
> 

> **Example 2**: 
>
> Input: [[1,2,3],[4,5,6]]
> Output: [[1,4],[2,5],[3,6]]

> **Note:**
>
> - `1 <= A.length <= 1000`
> - `1 <= A[0].length <= 1000`

---

### Approach 1: Copy Directly

**Intuition and Algorithm**

The transpose of a matrix `A` with dimensions `R x C` is a matrix `ans` with dimensions `C x R` for which `ans[c][r] = A[r][c]`.

Let's initialize a new matrix `ans` representing the answer. Then, we'll copy each entry of the matrix as appropriate.

```python
class Solution:
    def transpose(self, A):
        res = []
        for i in range(len(A[0])):
            temp = []
            for j in range(len(A)):
                temp.append(A[j][i])
            res.append(temp)
        return res

def main():
    A = [[1,2,3],[4,5,6],[7,8,9],[10,11,12]]
    answer=Solution()
    results=answer.transpose(A)
    print(results)


if __name__ == "__main__":
    main()
```

