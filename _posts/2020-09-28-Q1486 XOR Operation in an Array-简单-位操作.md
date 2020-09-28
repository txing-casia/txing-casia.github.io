---
layout:     post
title:      "Q1486 XOR Operation in an Array-简单-位操作"
subtitle:   ""
date:       2020-09-28
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
---

# [XOR Operation in an Array](https://leetcode-cn.com/problems/xor-operation-in-an-array/)

## Question

> Given an integer n and an integer start.
>
> Define an array nums where nums[i] = start + 2*i (0-indexed) and n == nums.length.
>
> Return the bitwise XOR of all elements of nums.
>
> **Constraints:**
>
> - `1 <= n <= 1000`
> - `0 <= start <= 1000`
> - `n == nums.length`

> **Example 1:**
>
> Input: n = 5, start = 0
> Output: 8
> Explanation: Array nums is equal to [0, 2, 4, 6, 8] where (0 ^ 2 ^ 4 ^ 6 ^ 8) = 8.
>    Where "^" corresponds to bitwise XOR operator.

> **Example 2:**
>
> Input: n = 4, start = 3
> Output: 8
> Explanation: Array nums is equal to [3, 5, 7, 9] where (3 ^ 5 ^ 7 ^ 9) = 8.

> **Example 3:**
>
> Input: n = 1, start = 7
> Output: 7

### Approach 1: 位操作

```python
class Solution(object):
    def xorOperation(self, n: int, start: int) -> int:
        nums=0
        for i in range(n):
            nums ^= (start+2*i)
        return nums

def main():
    n=4
    start=3
    solution=Solution().xorOperation(n,start)
    print(solution)

if __name__ == "__main__":
    main()
```

**复杂度分析**

- 时间复杂度：$$O(n)$$

- 空间复杂度：$$O(1)$$

  
