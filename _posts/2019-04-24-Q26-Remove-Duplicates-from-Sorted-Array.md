---
layout:     post
title:      "Q26 Remove Duplicates from Sorted Array"
subtitle:   ""
date:       2019-04-24
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Array
---

# [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

## Question

> Given a sorted array *nums*, remove the duplicates in-place such that each element appear only *once* and return the new length.
>
> Do not allocate extra space for another array, you must do this by **modifying the input array in-place** with O(1) extra memory.

> **Example 1**: 
>
> Given nums = [1,1,2],
>
> Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
>
> It doesn't matter what you leave beyond the returned length.

> **Example 2**: 
>
> Given nums = [0,0,1,1,1,2,2,3,3,4],
>
> Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.
>
> It doesn't matter what values are set beyond the returned length.

>**Clarification:**
>
>Confused why the returned value is an integer but your answer is an array?
>
>Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.
>
>Internally you can think of this:
>
>```c++
>// nums is passed in by reference. (i.e., without making a copy)
>int len = removeDuplicates(nums);
>
>// any modification to nums in your function would be known by the caller.
>// using the length returned by your function, it prints the first len elements.
>for (int i = 0; i < len; i++) {
>    print(nums[i]);
>}
>```



## Solution 

### Approach 1: Two Pointers

Since the array is already sorted, we can keep two pointers $i$ and $j$, where $i$ is the slow-runner while $j$ is the fast-runner. As long as $$nums[i] = nums[j]$$, we increment $j$ to skip the duplicate.

When we encounter $nums[j]\neq nums[i]$, the duplicate run has ended so we must copy its value to $nums[i + 1]$.  $i$ is then incremented and we repeat the same process again $j​$ reaches the end of array.

```python
class Solution:
    def removeDuplicates(self, nums: 'List[int]') -> 'int':
        if len(nums)==0:
            return 0
        else:
            index=0
            while(index+1<len(nums)):
                if nums[index]==nums[index+1]:
                    nums.remove(nums[index])
                else:
                    index+=1
        return len(nums)

if __name__ == "__main__":
    nums = [2,2,2,3,4]
    answer=Solution()
    results=answer.removeDuplicates(nums)
    print(results)
   
```

- Time complextiy : *O*(*n*). Assume that *n* is the length of array. Each of i*i* and *j* traverses at most n*n*steps.
- Space complexity : *O*(1).

代码中使用了nums.remove() 来删除重复元素，.pop() 也有类似效果，但在提交LeetCode时候超时未通过。