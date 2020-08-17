---
layout:     post
title:      "Q961 N-Repeated Element in Size 2N Array"
subtitle:   "(Original Answer) Runtime: 48 ms, faster than 85.41% of Python3 online submissions"
date:       2019-05-07
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Hash Table
---

# [N-Repeated Element in Size 2N Array](https://leetcode.com/problems/n-repeated-element-in-size-2n-array/)

## Question

> In a array `A` of size `2N`, there are `N+1` unique elements, and exactly one of these elements is repeated N times.
>
> Return the element repeated `N` times.

> **Example 1**: 
>
> Input: [1,2,3,3]
> Output: 3

> **Example 2**: 
>
> Input: [2,1,2,5,3,2]
> Output: 2

> **Example 3**: 
>
> Input: [5,1,5,2,5,3,5,4]
> Output: 5

> **Note**:
>
> 1. `4 <= A.length <= 10000`
> 2. `0 <= A[i] < 10000`
> 3. `A.length` is even



### Approach 1: Hash Table (Original Answer)

**Intuition and Algorithm**

Using Hash table to distinguish repeated element in array `A`. 

```python
class Solution:
    def repeatedNTimes(self, A):
        Hashmap=dict()
        # 建立Hash Table
        for i in range(0, len(A)):
            if not(A[i] in Hashmap):
                Hashmap[A[i]] = i #键是数值，值是序号
            else:
                return A[i]

def main():
    A =[5,1,5,2,5,3,5,4]
    answer=Solution()
    results=answer.repeatedNTimes(A)
    print(results)


if __name__ == "__main__":
    main()
```

- Runtime: 48 ms, faster than 85.41% of Python3 online submissions for N-Repeated Element in Size 2N Array.
- Memory Usage: 14.1 MB, less than 5.12% of Python3 online submissions for N-Repeated Element in Size 2N Array.