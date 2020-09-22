---
layout:     post
title:      "Q1470 Shuffle the Array-简单"
subtitle:   ""
date:       2020-09-22
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
---

# [Number of Good Pairs](https://leetcode-cn.com/problems/number-of-good-pairs/)

## Question

> Given an array of integers nums.
>
> A pair (i,j) is called good if nums[i] == nums[j] and i < j.
>
> Return the number of good pairs.
>
> **Constraints:**
>
> - `1 <= nums.length <= 100`
> - `1 <= nums[i] <= 100`

> **Example 1:**
>
> Input: nums = [1,2,3,1,1,3]
> Output: 4
> Explanation: There are 4 good pairs (0,3), (0,4), (3,4), (2,5) 0-indexed.

> **Example 2:**
>
> Input: nums = [1,1,1,1]
> Output: 6
> Explanation: Each pair in the array are good.

> **Example 3:**
>
> Input: nums = [1,2,3]
> Output: 0

### Approach 1: 循环嵌套
过于简单，直接上代码：

```python
class Solution(object):
    def numIdenticalPairs(self, nums):
        if not nums:
            return
        n=len(nums)
        ans=0
        for i in range(n):
            for j in range(i+1,n):
                if nums[i]==nums[j]:
                    ans+=1
        return ans

def main():
    nums =[1,2,3,1,1,3]
    solution=Solution().numIdenticalPairs(nums)
    print(solution)

if __name__ == "__main__":
    main()
```

**复杂度分析**

- 时间复杂度：$$O(\frac{N(N+1)}{2})$$

- 空间复杂度：O(1)

## Approach 2: 组合计数

用哈希表统计每个数在序列中出现的次数，假设数字 k 在序列中出现的次数为 v，那么满足题目中所说的 $${\rm nums}[i] = {\rm nums}[j] = k(i < j)$$ 的 $$(i, j)$$ 的数量就是 $$\frac{v(v - 1)}{2} $$，即 k 这个数值对答案的贡献是 $$\frac{v(v - 1)}{2}$$。我们只需要把所有数值的贡献相加，即可得到答案。

```python
class Solution:
    def numIdenticalPairs(self, nums: List[int]) -> int:
        m = collections.Counter(nums)
        return sum(v * (v - 1) // 2 for k, v in m.items())
```

**复杂度分析**

- 时间复杂度：O*(*n)。
- 空间复杂度：O*(*n)，即哈希表使用到的辅助空间的空间代价。