---
layout:     post
title:      "Q977 Squares of a Sorted Array"
subtitle:   ""
date:       2019-06-03
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - String
---

# [Squares of a Sorted Array](<https://leetcode.com/problems/squares-of-a-sorted-array/>)

## Question 

> Given an array of integers `A` sorted in non-decreasing order, return an array of the squares of each number, also in sorted non-decreasing order.

> **Example 1:**
> Input: [-4,-1,0,3,10]
> Output: [0,1,9,16,100]

> **Example 2:**
> Input: [-7,-3,2,3,11]
> Output: [4,9,9,49,121]

### Approach 1: Direct Method

```python
class Solution(object):
    def sortedSquares(self, A):
        B=[]
        for i in range(0,len(A)):
            B.append(int(math.pow(A[i],2)))
        B.sort()
        return B

def main():
    Ain=[-4,-1,0,3,10]
    solution=Solution().sortedSquares(Ain)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Runtime: 220 ms, faster than 33.27% of Python online submissions forSquares of a Sorted Array.
- Memory Usage: 13.9 MB, less than 20.54% of Python online submissions for Squares of a Sorted Array.

## Approach 2: Sort

```python
class Solution(object):
    def sortedSquares(self, A):
        return sorted(x*x for x in A)
```

**Complexity Analysis**

- Time Complexity: *O*(*N*log*N*), where *N* is the length of `A`.
- Space Complexity: *O*(*N*). 

- Runtime: 200 ms, faster than 58.97% of Python online submissions for Squares of a Sorted Array.

- Memory Usage: 13.9 MB, less than 25.48% of Python online submissions for Squares of a Sorted Array.