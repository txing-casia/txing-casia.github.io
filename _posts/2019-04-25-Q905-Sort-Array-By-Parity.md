---
layout:     post
title:      "Q905 Sort Array By Parity"
subtitle:   ""
date:       2019-04-25
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
---

# [Sort Array By Parity](https://leetcode.com/problems/sort-array-by-parity/)

## Question

> Given an array `A` of non-negative integers, return an array consisting of all the even elements of `A`, followed by all the odd elements of `A`.
>
> You may return any answer array that satisfies this condition.

> **Example 1**: 
>
> Input: [3,1,2,4]
> Output: [2,4,3,1]
> The outputs [4,2,3,1], [2,4,1,3], and [4,2,1,3] would also be accepted.



## Solution 

### Approach 1: Sort

Since the array is already sorted, we can keep two pointers $i$ and $j$, where $i$ is the slow-runner while $j$ is the fast-runner. As long as $$nums[i] = nums[j]$$, we increment $j$ to skip the duplicate.

When we encounter $nums[j]\neq nums[i]$, the duplicate run has ended so we must copy its value to $nums[i + 1]$.  $i$ is then incremented and we repeat the same process again $j​$ reaches the end of array.

```python
class Solution:
    def sortArrayByParity(self, A):
        even=[]
        odd=[]
        for i in range(0,len(A)):
            if A[i]%2==0:
                even.append(A[i])
            else:
                odd.append(A[i])
        return even+odd


if __name__ == "__main__":
    nums = [3,1,2,4]
    answer=Solution()
    results=answer.sortArrayByParity(nums)
    print(results)
```

- Runtime: 68 ms, faster than 83.75% of Python3 online submissions for Sort Array By Parity.
- Memory Usage: 13.8 MB, less than 5.69% of Python3 online submissions for Sort Array By Parity.

Use a custom comparator when sorting, to sort by parity.

```python
class Solution(object):
    def sortArrayByParity(self, A):
        A.sort(key = lambda x: x % 2)
        return A
```

- Time Complexity: *O*(*N*log*N*), where *N* is the length of `A`.
- Space Complexity: *O*(*N*) for the sort, depending on the built-in implementation of `sort`.

### Approach 2: Two Pass

Write all the even elements first, then write all the odd elements.

```python
class Solution(object):
    def sortArrayByParity(self, A):
        return ([x for x in A if x % 2 == 0] +
                [x for x in A if x % 2 == 1])
```

- Time Complexity: *O*(*N*), where *N* is the length of `A`.
- Space Complexity: *O*(*N*) for the sort, depending on the built-in implementation of `sort`. 

### Approach 3: In-Place

**Intuition**

If we want to do the sort in-place, we can use **quicksort**, a standard textbook algorithm.

**Algorithm**

​	We'll maintain two pointers `i` and `j`. The loop invariant is everything below `i` has parity `0` (ie. `A[k] % 2 == 0` when `k < i`), and everything above `j` has parity `1`.

Then, there are 4 cases for `(A[i] % 2, A[j] % 2)`:

- If it is `(0, 1)`, then everything is correct: `i++` and `j--`.
- If it is `(1, 0)`, we swap them so they are correct, then continue.
- If it is `(0, 0)`, only the `i` place is correct, so we `i++` and continue.
- If it is `(1, 1)`, only the `j` place is correct, so we `j--` and continue.

Throughout all 4 cases, the loop invariant is maintained, and `j-i` is getting smaller. So eventually we will be done with the array sorted as desired.

```python
class Solution(object):
    def sortArrayByParity(self, A):
        i, j = 0, len(A) - 1
        while i < j:
            if A[i] % 2 > A[j] % 2:
                A[i], A[j] = A[j], A[i] # 直接交换，不用中间变量

            if A[i] % 2 == 0: i += 1
            if A[j] % 2 == 1: j -= 1

        return A
```

- Time Complexity: *O*(*N*), where *N* is the length of `A`. Each step of the while loop makes `j-i`decrease by at least one. (Note that while quicksort is *O*(*N*log*N*) normally, this is *O*(*N*)because we only need one pass to sort the elements.)
- Space Complexity: *O*(1) in additional space complexity. 