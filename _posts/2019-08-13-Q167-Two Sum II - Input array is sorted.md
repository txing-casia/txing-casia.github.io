---
layout:     post
title:      "Q167 Two Sum II - Input array is sorted"
subtitle:   ""
date:       2019-08-13
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - Binary search
---

# [Two Sum II - Input array is sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

## Question 

> Given an array of integers that is already **sorted in ascending order**, find two numbers such that they add up to a specific target number.
>
> The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2.

> **Example 1:**
>
> Input: numbers = [2,7,11,15], target = 9
> Output: [1,2]
> Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.

> **Note:**
>
> - Your returned answers (both index1 and index2) are not zero-based.
> - You may assume that each input would have *exactly* one solution and you may not use the same element twice.

## Approach 1: Dictionary           

**Intuition and Algorithm**

```python
class Solution(object):
    def twoSum(self, numbers, target):
        dic = {}
        for i, num in enumerate(numbers): # 获得编号i(从0开始)和值num
            if target - num in dic:
                return [dic[target - num] + 1, i + 1]
            dic[num] = i


def main():
    numbers = [2, 7, 11, 15]
    target = 9
    solution=Solution().twoSum(numbers,target)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: *O*(*N*), where *N* is the length of `numbers`.
- Space Complexity: *O*(1).
- Runtime: 40 ms, faster than 98.01% of Python online submissions for Two Sum II - Input array is sorted.
- Memory Usage: 12.1 MB, less than 40.00% of Python online submissions for Two Sum II - Input array is sorted.
