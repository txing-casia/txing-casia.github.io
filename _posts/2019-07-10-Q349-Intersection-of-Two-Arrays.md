---
layout:     post
title:      "Q349 Intersection of Two Arrays"
subtitle:   ""
date:       2019-07-10
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - Binary search
---

# [Intersection of Two Arrays](https://leetcode.com/problems/peak-index-in-a-mountain-array/)

## Question 

> Given two arrays, write a function to compute their intersection.

> **Example 1:**
>
> Input: nums1 = [1,2,2,1], nums2 = [2,2]
> Output: [2]

> **Example 2:**
>
> Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
> Output: [9,4]

> **Note:**
>
> - Each element in the result must be unique.
> - The result can be in any order.

## Approach 1: Array

**Intuition and Algorithm**

```python
class Solution(object):
    def intersection(self, nums1, nums2):
        out=[]
        for A in nums1:
            if A in nums2 and A not in out:
                out.append(A)
        return out

def main():
    nums1 = [4, 9, 5]
    nums2 = [9, 4, 9, 8, 4]
    solution=Solution().intersection(nums1,nums2)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: *O*(*N*), where *N* is the length of `nums1`.
- Space Complexity: *O*(1).
- Runtime: 52 ms, faster than 23.52% of Python online submissions for Intersection of Two Arrays.
- Memory Usage: 11.8 MB, less than 86.37% of Python online submissions for Intersection of Two Arrays.



## Approach 2: Binary Search

**Intuition and Algorithm**

The comparison `A[i] < A[i+1]` in a mountain array looks like `[True, True, True, ..., True, False, False, ..., False]`: 1 or more boolean `True`s, followed by 1 or more boolean `False`. For example, in the mountain array `[1, 2, 3, 4, 1]`, the comparisons `A[i] < A[i+1]` would be `True, True, True, False`.

We can binary search over this array of comparisons, to find the largest index `i` such that `A[i] < A[i+1]`. For more on *binary search*, see the [LeetCode explore topic here.](https://leetcode.com/explore/learn/card/binary-search/)

```python
class Solution(object):
    def intersection(self, nums1, nums2):
        set1 = set(nums1)
        set2 = set(nums2)
        return list(set2 & set1)


def main():
    nums1 = [4, 9, 5]
    nums2 = [9, 4, 9, 8, 4]
    solution=Solution().intersection(nums1,nums2)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time complexity : $$\mathcal{O}(n + m)$$ in the average case and $$\mathcal{O}(n \times m)$$ [in the worst case when load factor is high enough](https://wiki.python.org/moin/TimeComplexity#set).
- Space complexity : $$\mathcal{O}(n + m)$$ in the worst case when all elements in the arrays are different.
- Runtime: 28 ms, faster than 93.60% of Python online submissions for Intersection of Two Arrays.
- Memory Usage: 11.9 MB, less than 50.78% of Python online submissions for Intersection of Two Arrays.