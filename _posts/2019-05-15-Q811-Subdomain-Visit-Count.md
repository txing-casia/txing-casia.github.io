---
layout:     post
title:      "Q811 Subdomain Visit Count"
subtitle:   ""
date:       2019-05-14
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Hash Table
    - Easy
---

# [Subdomain Visit Count](<https://leetcode.com/problems/subdomain-visit-count/>)

## Question

> A website domain like "discuss.leetcode.com" consists of various subdomains. At the top level, we have "com", at the next level, we have "leetcode.com", and at the lowest level, "discuss.leetcode.com". When we visit a domain like "discuss.leetcode.com", we will also visit the parent domains "leetcode.com" and "com" implicitly.
>
> Now, call a "count-paired domain" to be a count (representing the number of visits this domain received), followed by a space, followed by the address. An example of a count-paired domain might be "9001 discuss.leetcode.com".
>
> We are given a list `cpdomains` of count-paired domains. We would like a list of count-paired domains, (in the same format as the input, and in any order), that explicitly counts the number of visits to each subdomain.

> **Example 1:**
> Input: 
> ["9001 discuss.leetcode.com"]
> Output: 
> ["9001 discuss.leetcode.com", "9001 leetcode.com", "9001 com"]
> Explanation: 
> We only have one website domain: "discuss.leetcode.com". As discussed above, the subdomain "leetcode.com" and "com" will also be visited. So they will all be visited 9001 times.

> **Example 2:**
> Input: 
> ["900 google.mail.com", "50 yahoo.com", "1 intel.mail.com", "5 wiki.org"]
> Output: 
> ["901 mail.com","50 yahoo.com","900 google.mail.com","5 wiki.org","5 org","1 intel.mail.com","951 com"]
> Explanation: 
> We will visit "google.mail.com" 900 times, "yahoo.com" 50 times, "intel.mail.com" once and "wiki.org" 5 times. For the subdomains, we will visit "mail.com" 900 + 1 = 901 times, "com" 900 + 50 + 1 = 951 times, and "org" 5 times.

> **Note:**
>
> 1. You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
> 2. Your algorithm's time complexity must be better than O(n log n), where n is the array's size.

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





