---
layout:     post
title:      "Q557 Reverse Words in a String III"
subtitle:   ""
date:       2019-06-10
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - String
---

# [Reverse Words in a String III](<https://leetcode.com/problems/reverse-words-in-a-string-iii/>)

## Question 

> Given a string, you need to reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

> **Example 1:**
>
> Input: "Let's take LeetCode contest"
> Output: "s'teL ekat edoCteeL tsetnoc"

> **Note:**
>
> In the string, each word is separated by single space and there will not be any extra space in the string.

## Approach 1: Canonical Form

**Intuition and Algorithm**

For each email address, convert it to the *canonical* address that actually receives the mail. This involves a few steps:

- Separate the email address into a `local` part and the `rest` of the address.
- If the `local` part has a `'+'` character, remove it and everything beyond it from the `local` part.
- Remove all the zeros from the `local` part.
- The canonical address is `local + rest`.

After, we can count the number of unique canonical addresses with a `Set` structure.

```python
class Solution(object):
    def numUniqueEmails(self, emails):
        seen = set()
        for email in emails:
            local, domain = email.split('@')
            if '+' in local:
                local = local[:local.index('+')]
            seen.add(local.replace('.','') + '@' + domain)
        return len(seen)


def main():
    Emails=["test.email+alex@leetcode.com",
            "test.e.mail+bob.cathy@leetcode.com",
            "testemail+david@lee.tcode.com"]
    solution=Solution().numUniqueEmails(Emails)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: *O*(*n*), where *n* is the total content of `emails`.
- Space Complexity: *O*(*n*).
- Runtime: 28 ms, faster than 99.72% of Python online submissions for Unique Email Addresses.
- Memory Usage: 11.7 MB, less than 92.68% of Python online submissions for Unique Email Addresses.
