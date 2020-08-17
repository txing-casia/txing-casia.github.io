---
layout:     post
title:      "Q917 Reverse Only Letters"
subtitle:   ""
date:       2019-06-19
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - String
---

# [Reverse Only Letters](https://leetcode.com/problems/reverse-only-letters/)

## Question 

> Given a string `S`, return the "reversed" string where all characters that are not a letter stay in the same place, and all letters reverse their positions.

> **Example 1:**
>
> Input: "ab-cd"
> Output: "dc-ba"

> **Example 2:**
>
> Input: "a-bC-dEf-ghIj"
> Output: "j-Ih-gfE-dCba"

> **Example 3:**
>
> Input: "Test1ng-Leet=code-Q!"
> Output: "Qedo1ct-eeLg=ntse-T!"

> **Note:**
>
> - S.length <= 100
> - 33 <= S[i].ASCIIcode <= 122 
> - S doesn't contain `\` or `"`

## Approach 1: Stack of Letters

**Intuition and Algorithm**

Collect the letters of `S` separately into a stack, so that popping the stack reverses the letters. (Alternatively, we could have collected the letters into an array and reversed the array.)

Then, when writing the characters of `S`, any time we need a letter, we use the one we have prepared instead.

```python
class Solution(object):
    def reverseOnlyLetters(self, S):
        letters = [c for c in S if c.isalpha()]
        ans = []
        for c in S:
            if c.isalpha():
                ans.append(letters.pop())
            else:
                ans.append(c)
        return "".join(ans)


def main():
    S="ab-cd"
    solution=Solution().reverseOnlyLetters(S)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity:*O*(*N*), where *N* is the length of `S`.
- Space Complexity: *O*(*N*). 
- Runtime: 16 ms, faster than 92.70% of Python online submissions for Reverse Only Letters.
- Memory Usage: 11.8 MB, less than 51.06% of Python online submissions for Reverse Only Letters.