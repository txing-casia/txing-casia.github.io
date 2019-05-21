---
layout:     post
title:      "Q136 Single Number"
subtitle:   ""
date:       2019-05-16
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Hash Table
    - Easy
---

# [Single Number](<https://leetcode.com/problems/single-number/>)

## Question

> Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

> **Example 1:**
> Input: [2,2,1]
> Output: 1

> **Example 2:**
> Input: [4,1,2,1,2]
> Output: 4

> **Note:**
>
> Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

### Approach 1: Math

**Concept**

$$2 * (a + b + c) - (a + a + b + b + c) = c$$

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
		return 2 * sum(set(nums)) - sum(nums)
```

- Runtime: 32 ms, faster than 99.87% of Python3 online submissions for Single Number.
- Memory Usage: 15.1 MB, less than 14.91% of Python3 online submissions for Single Number.

### Approach 2: Hash Table

```python
class Solution:
    def singleNumber(self, nums):
        hash_table = {} # 等同 hash_table=dict()
        for i in nums:
            try:
                hash_table.pop(i) # 如果字典为空，转入异常状态
            except:
                hash_table[i] = 1 # 填充hash表
        return hash_table.popitem()[0]
```

- Runtime: 44 ms, faster than 68.21% of Python3 online submissions for Single Number.
- Memory Usage: 15.1 MB, less than 12.23% of Python3 online submissions for Single Number.

