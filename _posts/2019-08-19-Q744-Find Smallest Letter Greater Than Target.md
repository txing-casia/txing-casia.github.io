---
layout:     post
title:      "Q744 Find Smallest Letter Greater Than Target"
subtitle:   ""
date:       2019-08-19
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - Binary search
---

# [Find Smallest Letter Greater Than Target](https://leetcode.com/problems/find-smallest-letter-greater-than-target/)

## Question 

> Given a list of sorted characters `letters` containing only lowercase letters, and given a target letter `target`, find the smallest element in the list that is larger than the given target.
>
> Letters also wrap around. For example, if the target is `target = 'z'` and `letters = ['a', 'b']`, the answer is `'a'`.

> **Example 1:**
>
> Input:
> letters = ["c", "f", "j"]
> target = "a"
> Output: "c"
>
> Input:
> letters = ["c", "f", "j"]
> target = "c"
> Output: "f"
>
> Input:
> letters = ["c", "f", "j"]
> target = "d"
> Output: "f"
>
> Input:
> letters = ["c", "f", "j"]
> target = "g"
> Output: "j"
>
> Input:
> letters = ["c", "f", "j"]
> target = "j"
> Output: "c"
>
> Input:
> letters = ["c", "f", "j"]
> target = "k"
> Output: "c"

> **Note:**
>
> - `letters` has a length in range `[2, 10000]`.
> - `letters` consists of lowercase letters, and contains at least 2 unique letters.
> - `target` is a lowercase letter.

## Approach 1: Linear Search        

**Intuition and Algorithm**

```python
class Solution(object):
    def nextGreatestLetter(self, letters, target):
        for l in letters:
            if l > target:
                return l
        return letters[0]

def main():
    letters = ["c", "f", "j"]
    target = "j"
    solution=Solution().nextGreatestLetter(letters,target)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time complexity : $$\mathcal{O}(N)$$.
- Space complexity : $$\mathcal{O}(1)​$$ since it's a constant space solution.
- Runtime: 72 ms, faster than 99.40% of Python online submissions for Find Smallest Letter Greater Than Target.
- Memory Usage: 13.8 MB, less than 33.33% of Python online submissions for Find Smallest Letter Greater Than Target.
