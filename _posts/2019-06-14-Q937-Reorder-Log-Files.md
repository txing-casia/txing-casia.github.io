---
layout:     post
title:      "Q937 Reorder Log Files"
subtitle:   ""
date:       2019-06-14
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - String
---

# [Reorder Log Files](https://leetcode.com/problems/reorder-log-files/)

## Question 

> You have an array of `logs`.  Each log is a space delimited string of words.
>
> For each log, the first word in each log is an alphanumeric *identifier*.  Then, either:
>
> - Each word after the identifier will consist only of lowercase letters, or;
> - Each word after the identifier will consist only of digits.
>
> We will call these two varieties of logs *letter-logs* and *digit-logs*.  It is guaranteed that each log has at least one word after its identifier.
>
> Reorder the logs so that all of the letter-logs come before any digit-log.  The letter-logs are ordered lexicographically ignoring identifier, with the identifier used in case of ties.  The digit-logs should be put in their original order.
>
> Return the final order of the logs.

> **Example 1:**
>
> Input: ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]
> Output: ["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]

> **Note:**
>
> - 0 <= logs.length <= 100
> - 3 <= logs[i].length <= 100
> - logs[i] is guaranteed to have an identifier, and a word after the identifier.

## Approach 1: Custom Sort

**Intuition and Algorithm**

Instead of sorting in the default order, we'll sort in a custom order we specify.

The rules are:

- Letter-logs come before digit-logs;
- Letter-logs are sorted alphanumerically, by content then identifier;
- Digit-logs remain in the same order.

It is straightforward to translate these ideas into code.

```python
class Solution(object):
    def reorderLogFiles(self, logs):
        def f(log):
            id_, rest = log.split(" ", 1) # 1的意思是按空格阶段分成2段，
                                          # 前面n-1段各自只有1个元素
            return (0, rest, id_) if rest[0].isalpha() else (1,)
                                          # 检察rest[0]是不是字符

        return sorted(logs, key = f)


def main():
    S="a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"
    solution=Solution().reorderLogFiles(S)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: $$O(\mathcal{A}\log \mathcal{A})$$, where $$\mathcal{A}\log \mathcal{A}$$ is the total content of `logs`.
- Space Complexity: $$O(\mathcal{A})$$.

- Runtime: 36 ms, faster than 20.01% of Python online submissions for Reorder Log Files.
- Memory Usage: 12 MB, less than 32.91% of Python online submissions for Reorder Log Files.