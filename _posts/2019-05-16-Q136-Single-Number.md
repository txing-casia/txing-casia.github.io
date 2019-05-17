---
layout:     post
title:      "Q136 Single Number"
subtitle:   ""
date:       2019-05-16
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Hash Table
    - Easy
---

# [Single Number](<https://leetcode.com/problems/single-number/>)

## Question

> Given a **non-empty** array of integers, every element appears *twice* except for one. Find that single one.

> **Example 1:**
> Input: [2,2,1]
> Output: 1

> **Example 2:**
> Input: [4,1,2,1,2]
> Output: 4

> **Note:**
>
> 1. Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

### Approach 1: Hash map

**Intuition and Algorithm**

The algorithm is straightforward: we just do what the problem statement tells us to do.

For an address like `a.b.c`, we will count `a.b.c`, `b.c`, and `c`. For an address like `x.y`, we will count `x.y` and `y`.

To count these strings, we will use a hash map. To split the strings into the required pieces, we will use library `split` functions.

```python
import collections

class Solution(object):
    def subdomainVisits(self, cpdomains):
        ans = collections.Counter()
        for domain in cpdomains:
            count, domain = domain.split() # 用空格分割字符串
            count = int(count)
            frags = domain.split('.') # 用点分割域名
            for i in range(len(frags)):
                ans[".".join(frags[i:])] += count # join()连接字符数组 

        return ["{} {}".format(ct, dom) for dom, ct in ans.items()]

def main():
    cpdomains =["9001 discuss.leetcode.com"]
    answer=Solution()
    results=answer.subdomainVisits(cpdomains)
    print(results)


if __name__ == "__main__":
    main()
```

- Runtime: 52 ms, faster than 99.76% of Python3 online submissions for Subdomain Visit Count.
- Memory Usage: 13.3 MB, less than 7.07% of Python3 online submissions for Subdomain Visit Count.





