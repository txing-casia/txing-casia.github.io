---
layout:     post
title:      "Q704 Binary Search"
subtitle:   ""
date:       2019-08-14
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - Binary search
---

# [Binary Search](https://leetcode.com/problems/binary-search/)

## Question 

> Given a **sorted** (in ascending order) integer array `nums` of `n` elements and a `target` value, write a function to search `target` in `nums`. If `target` exists, then return its index, otherwise return `-1`.

> **Example 1:**
>
> Input: nums = [-1,0,3,5,9,12], target = 9
> Output: 4
> Explanation: 9 exists in nums and its index is 4

> **Example 2:**
>
> Input: nums = [-1,0,3,5,9,12], target = 2
> Output: -1
> Explanation: 2 does not exist in nums so return -1

> **Note:**
>
> - You may assume that all elements in `nums` are unique.
> - `n` will be in the range `[1, 10000]`.
> - The value of each element in `nums` will be in the range `[-9999, 9999]`.s

## Approach 1: Binary Search        

**Intuition and Algorithm**

```python
class Solution(object):
    def search(self, nums, target):
        left, right = 0, len(nums) - 1 # left和right分别是首末两个元素
        while left <= right: # 不断移动左右边界
            pivot = (left + right) // 2 # 取中间序号并观察大小
            if nums[pivot] == target:
                return pivot
            else:
                if target < nums[pivot]:
                    right = pivot - 1
                else:
                    left = pivot + 1
        return -1


def main():
    nums = [-1, 0, 3, 5, 9, 12]
    target = 9
    solution=Solution().search(nums,target)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time complexity : $$\mathcal{O}(\log N)$$.
- Let's compute time complexity with the help of [master theorem](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)) $$T(N) = aT\left(\frac{N}{b}\right) + \Theta(N^d)$$. The equation represents dividing the problem up into $$a$$ subproblems of size $$\frac{N}{b}$$ in $$\Theta(N^d)$$ time. Here at step there is only one subproblem `a = 1`, its size is a half of the initial problem `b = 2`, and all this happens in a constant time `d = 0`. That means that $$\log_b{a} = d$$ and hence we're dealing with [case 2](https://en.wikipedia.org/wiki/Master_theorem_(analysis_of_algorithms)#Case_2_example) that results in $$\mathcal{O}(n^{\log_b{a}} \log^{d + 1} N)=O(\log N) $$time complexity.
- Space complexity : $$\mathcal{O}(1)$$ since it's a constant space solution.
- Runtime: 236 ms, faster than 11.72% of Python online submissions for Binary Search.
- Memory Usage: 12.8 MB, less than 33.33% of Python online submissions for Binary Search.
