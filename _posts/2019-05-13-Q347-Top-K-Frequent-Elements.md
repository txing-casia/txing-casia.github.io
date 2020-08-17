---
layout:     post
title:      "Q347 Top K Frequent Elements"
subtitle:   ""
date:       2019-05-13
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Hash Table
---

# [Top K Frequent Elements](<https://leetcode.com/problems/top-k-frequent-elements/>)

## Question

> Given a non-empty array of integers, return the **k** most frequent elements.

> **Example 1:**
>
> Input: nums = [1,1,1,2,2,3], k = 2
> Output: [1,2]

> **Example 2:**
>
> Input: nums = [1], k = 1
> Output: [1]

> **Note:**
>
> 1. You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
> 2. Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

### Approach 1: Heap

**Intuition and Algorithm**

If `k = 1` the linear-time solution is quite simple. One could keep the frequency of elements appearance in a hash map and update the maximum element at each step.

When `k > 1` we need a data structure that has a fast access to the elements ordered by their frequencies. The idea here is to use the heap which is also known as priority queue. 

The first step is to build a hash map `element -> its frequency`. In Java we could use data structure `HashMap` but have to fill it manually. Python provides us both a dictionary structure for the hash map and a method `Counter` in the `collections` library to build the hash map we need. 
This step takes $$\mathcal{O}(N)$$ time where `N` is number of elements in the list.

The second step is to build a heap. The time complexity of adding an element in a heap is $$\mathcal{O}(\log(k))$$ and we do it `N` times that means $$\mathcal{O}(N \log(k))$$ time complexity for this step.

The last step to build an output list has $$\mathcal{O}(k \log(k))$$ time complexity.

In Python there is a method `nlargest` in `heapq` library ([check here the source code](https://hg.python.org/cpython/file/2.7/Lib/heapq.py#l203)) which has the same $$\mathcal{O}(k \log(k))​$$ time complexity and combines two last steps in one line.

```python
import collections
import heapq

class Solution:
    def topKFrequent(self, nums, k):
        count = collections.Counter(nums)
        return heapq.nlargest(k, count.keys(), key=count.get)


def main():
    nums =[1,1,1,1,2,2,2,3,3,4]
    k=2
    answer=Solution()
    results=answer.topKFrequent(nums,k)
    print(results)


if __name__ == "__main__":
    main()
```

- Runtime: 40 ms, faster than 99.60% of Python3 online submissions for Top K Frequent Elements.
- Memory Usage: 15.8 MB, less than 8.41% of Python3 online submissions for Top K Frequent Elements.





