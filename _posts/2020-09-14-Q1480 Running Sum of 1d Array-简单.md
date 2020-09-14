---
layout:     post
title:      "Q1480 Running Sum of 1d Array-简单"
subtitle:   ""
date:       2020-09-14
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
---

# [Running Sum of 1d Array](https://leetcode-cn.com/problems/running-sum-of-1d-array/)

## Question

> Given an array nums. We define a running sum of an array as runningSum[i] = sum(nums[0]…nums[i]).
>
> Return the running sum of nums.
>
> **Constraints:**
>
> - `1 <= nums.length <= 1000`
> - `-10^6 <= nums[i] <= 10^6`

> **Example 1:**
>
> Input: nums = [1,2,3,4]
> Output: [1,3,6,10]
> Explanation: Running sum is obtained as follows: [1, 1+2, 1+2+3, 1+2+3+4].

> **Example 2:**
>
> Input: nums = [1,1,1,1,1]
> Output: [1,2,3,4,5]
> Explanation: Running sum is obtained as follows: [1, 1+1, 1+1+1, 1+1+1+1, 1+1+1+1+1].

> **Example 3:**
>
> Input: nums = [3,1,2,10,1]
> Output: [3,4,6,16,17]

### Approach 1: 
开始的时候想建立sum变量来储存前i项和，但这样造成了空间的浪费，可以直接改写input列表，将其作为输出。

**Note:** 如果面试遇到这个问题，问清楚面试官，是否可以修改传来的 nums 数组！

约束条件限制了nums非空，因此不用判断这点；限制了元素大小，因此不考虑传入错误字符的情况

```python
class Solution(object):
    def runningSum(self, nums):
        for i in range(1,len(nums)):
            nums[i]=nums[i-1]+nums[i]
        return nums


def main():
    nums = [1,1,1,1,1]
    solution=Solution().runningSum(nums)
    print(solution)


if __name__ == "__main__":
    main()
```

### 复杂度分析

- 时间复杂度O(n)
- 空间复杂度O(n)



