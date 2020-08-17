---
layout:     post
title:      "Q929 Unique Email Addresses"
subtitle:   ""
date:       2019-06-06
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - String
---

# [Unique Email Addresses](<https://leetcode.com/problems/unique-email-addresses/>)

## Question 

> Every email consists of a local name and a domain name, separated by the @ sign.
>
> For example, in `alice@leetcode.com`, `alice` is the local name, and `leetcode.com` is the domain name.
>
> Besides lowercase letters, these emails may contain `'.'`s or `'+'`s.
>
> If you add periods (`'.'`) between some characters in the **local name** part of an email address, mail sent there will be forwarded to the same address without dots in the local name.  For example, `"alice.z@leetcode.com"` and `"alicez@leetcode.com"` forward to the same email address.  (Note that this rule does not apply for domain names.)
>
> If you add a plus (`'+'`) in the **local name**, everything after the first plus sign will be **ignored**. This allows certain emails to be filtered, for example `m.y+name@email.com` will be forwarded to `my@email.com`.  (Again, this rule does not apply for domain names.)
>
> It is possible to use both of these rules at the same time.
>
> Given a list of `emails`, we send one email to each address in the list.  How many different addresses actually receive mails? 

> **Example 1:**
>
> Input: ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]
> Output: 2
> Explanation: "testemail@leetcode.com" and "testemail@lee.tcode.com" actually receive mails

> **Note:**
>
> - `1 <= emails[i].length <= 100`
> - `1 <= emails.length <= 100`
> - Each `emails[i]` contains exactly one `'@'` character.
> - All local and domain names are non-empty.
> - Local names do not start with a `'+'` character.

## Approach 1: Split Words  (Original Method)

**Intuition and Algorithm**

Note that `split` is a useful method.

```python
class Solution(object):
    def reverseWords(self, s):
        out = []
        singleWord=s.split(' ')
        for iter in singleWord:
            iter=iter[::-1]# 反向排序数组
            out.append(iter)
        return ' '.join(out) # 返回字符串，用空格填充out中元素的间隔

def main():
    s="Let's take LeetCode contest"
    solution=Solution().reverseWords(s)
    print(solution)


if __name__ == "__main__":
    main()
```

### simplification

```python
class Solution(object):
    def reverseWords(self, s):
        return " ".join([i[::-1] for i in s.split()])
```

**Complexity Analysis**

- Time Complexity: *O*(*n*), where *n* is the total words of `s`.
- Space Complexity: *O*(*1*).
- Runtime: 20 ms, faster than 94.03% of Python online submissions for Reverse Words in a String III.
- Memory Usage: 13 MB, less than 43.54% of Python online submissions for Reverse Words in a String III.

