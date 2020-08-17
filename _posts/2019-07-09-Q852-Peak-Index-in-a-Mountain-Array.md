---
layout:     post
title:      "Q852 Peak Index in a Mountain Array"
subtitle:   ""
date:       2019-07-09
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - Binary search
---

# [Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/)

## Question 

> Let's call an array `A` a *mountain* if the following properties hold:
>
> - `A.length >= 3`
> - There exists some `0 < i < A.length - 1` such that `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`
>
> Given an array that is definitely a mountain, return any `i` such that `A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]`.

> **Example 1:**
>
> Input: [0,1,0]
> Output: 1

> **Example 2:**
>
> Input: [0,2,1,0]
> Output: 1

> **Note:**
>
> - `3 <= A.length <= 10000`
> - `0 <= A[i] <= 10^6`
> - A is a mountain, as defined above.

## Approach 1: Linear Scan

**Intuition and Algorithm**

The mountain increases until it doesn't. The point at which it stops increasing is the peak.

```python
class Solution(object):
    def peakIndexInMountainArray(self, A):
        for i in range(0,len(A)):
            if A[i]>A[i+1]: break
        return i

def main():
    A=[0,1,2,0]
    solution=Solution().peakIndexInMountainArray(A)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: *O*(*N*), where *N* is the length of `A`.
- Space Complexity: *O*(1).
- Runtime: 56 ms, faster than 95.41% of Python online submissions for Peak Index in a Mountain Array.
- Memory Usage: 12.5 MB, less than 99.37% of Python online submissions for Peak Index in a Mountain Array.



## Approach 2: Binary Search

**Intuition and Algorithm**

The comparison `A[i] < A[i+1]` in a mountain array looks like `[True, True, True, ..., True, False, False, ..., False]`: 1 or more boolean `True`s, followed by 1 or more boolean `False`. For example, in the mountain array `[1, 2, 3, 4, 1]`, the comparisons `A[i] < A[i+1]` would be `True, True, True, False`.

We can binary search over this array of comparisons, to find the largest index `i` such that `A[i] < A[i+1]`. For more on *binary search*, see the [LeetCode explore topic here.](https://leetcode.com/explore/learn/card/binary-search/)

```python
class Solution(object):
    def peakIndexInMountainArray(self, A):
        lo, hi = 0, len(A) - 1
        while lo < hi:
            mi = (lo + hi) / 2
            if A[mi] < A[mi + 1]:
                lo = mi + 1
            else:
                hi = mi
        return lo

def main():
    A=[0,1,2,0]
    solution=Solution().peakIndexInMountainArray(A)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: *O*(*N*), where *N* is the length of `A`.
- Space Complexity: *O*(1).
- Runtime: 56 ms, faster than 95.41% of Python online submissions for Peak Index in a Mountain Array.
- Memory Usage: 12.5 MB, less than 99.37% of Python online submissions for Peak Index in a Mountain Array.