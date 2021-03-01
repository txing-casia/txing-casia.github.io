---
layout:     post
title:      "Q303 Range Sum Query(Immutable)-简单-前缀和"
subtitle:   ""
date:       2021-03-01
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
---

#### [Range Sum Query - Immutable](https://leetcode-cn.com/problems/range-sum-query-immutable/)

## Question

> Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.
>
> Implement the NumArray class:
>
> - NumArray(int[] nums) Initializes the object with the integer array nums.
> - int sumRange(int i, int j)  Return the sum of the elements of the nums array in the range [i, j] inclusive (i.e., sum(nums[i], nums[i + 1], ... , nums[j]))

> **Example 1:**
>
> Input
>["NumArray", "sumRange", "sumRange", "sumRange"]
>    [[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
>    Output
>    [null, 1, -1, -3]
>    
>    Explanation
>NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
> numArray.sumRange(0, 2); // return 1 ((-2) + 0 + 3)
> numArray.sumRange(2, 5); // return -1 (3 + (-5) + 2 + (-1)) 
>numArray.sumRange(0, 5); // return -3 ((-2) + 0 + 3 + (-5) + 2 + (-1))



### Approach 1: 前缀和

这道题题目写得看不出重点，实际想考察的是在大量重复调用的情况下，怎么把时间复杂度降到最小$$O(1)$$。因此需要在初始化的时候就对数据进行预处理，计算出前$$i$$项和：
$$
\begin{align}
sumRange(i,j)&=\sum_{k=i}^j nums[k]\\
&=\sum_{k=0}^j nums[k]-\sum_{k=0}^{i-1} nums[k]
\end{align}
$$


```python
class NumArray:

    def __init__(self, nums: List[int]):
        self.sums = [0]
        _sums = self.sums

        for num in nums:
            _sums.append(_sums[-1] + num)

    def sumRange(self, i: int, j: int) -> int:
        _sums = self.sums
        return _sums[j + 1] - _sums[i]
```

**复杂度分析**

- 时间复杂度：初始化 O(n)，每次检索 O(1)，其中 n 是数组 \textit{nums} 的长度。
  初始化需要遍历数组 \textit{nums} 计算前缀和，时间复杂度是 O(n)。
  每次检索只需要得到两个下标处的前缀和，然后计算差值，时间复杂度是 O(1)。

- 空间复杂度：O(n)，其中 n 是数组 \textit{nums} 的长度。需要创建一个长度为 n+1 的前缀和数组。