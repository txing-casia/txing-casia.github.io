---
layout:     post
title:      "JZOffer03-数组中重复的数字-简单-Hash Table"
subtitle:   ""
date:       2021-03-01
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - Hash Table
---

#### [剑指 Offer 03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

## Question

> 找出数组中重复的数字。
>
>
> 在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。
>

> **Example 1:**
>
> 输入：
>[2, 3, 1, 0, 2, 5, 3]
>    输出：2 或 3 



### Approach 1:  Hash table

这道题目最直接的做法当然是在一个For循环中判断是否存在某个数字，将其移除后在判断一次。但是直接这样写会导致时间复杂度比较高，需要对执行判断这部分代码进行优化。

最简单的应该就是使用 hash 表了，代码如下：


```python
class Solution:
    def findRepeatNumber(self, nums) -> int:
        Hash=dict()
        for i in range(len(nums)):
            if nums[i] not in Hash:
                Hash[nums[i]]=i
            else:
                return nums[i]
        return -1
    
    # 另一种方法
    def findRepeatNumber(self, nums) -> int: 
        temp_set = set()
        repeat = -1
        for i in range(len(nums)):
            temp_set.add(nums[i])
            if len(temp_set) < i + 1:
                repeat = nums[i]
                break
        return repeat
    
def main():
    s = [3,1,2,3]
    solution=Solution().findRepeatNumber(s)
    print(solution)

if __name__ == "__main__":
    main()
```

**复杂度分析**

- 时间复杂度：O(n)
  
  遍历数组一遍。使用哈希集合（HashSet），添加元素的时间复杂度为 O(1)，故总的时间复杂度是 O(n)。

- 空间复杂度：O(n)

  不重复的每个元素都可能存入集合，因此占用 O(n) 额外空间。