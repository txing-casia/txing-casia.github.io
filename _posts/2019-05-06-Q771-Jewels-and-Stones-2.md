---
layout:     post
title:      "Q771 Jewels and Stones"
subtitle:   "faster than 68.64% of Python3 online submissions for Jewels and Stones."
date:       2019-05-06
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Hash Table
---

# [Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/)

## Question

> You're given strings `J` representing the types of stones that are jewels, and `S` representing the stones you have.  Each character in `S` is a type of stone you have.  You want to know how many of the stones you have are also jewels.
>
> The letters in `J` are guaranteed distinct, and all characters in `J` and `S` are letters. Letters are case sensitive, so `"a"` is considered a different type of stone from `"A"`.

> **Example 1**: 
>
> Input: J = "aA", S = "aAAbbbb"
> Output: 3

> **Example 1**: 
>
> Input: J = "z", S = "ZZ"
> Output: 0

> **Note**:
>
> S and J will consist of letters and have length at most 50.
> The characters in J are distinct.



### Approach 1: Brute Force

**Intuition and Algorithm**

The brute force approach is simple. Loop through each element *i ,j* and find if they are equal.

```python
class Solution:
    def numJewelsInStones(self, J: str, S: str) -> int:
        num=0
        for i in J:
            for j in S:
                if i==j:
                    num+=1
        return num

def main():
    J = "aA"
    S="aAAbbbbb"
    answer=Solution()
    results=answer.numJewelsInStones(J,S)
    print(results)


if __name__ == "__main__":
    main()
```

- Runtime: 40 ms, faster than 68.64% of Python3 online submissions for Jewels and Stones.
- Memory Usage: 13.3 MB, less than 5.25% of Python3 online submissions for Jewels and Stones.

---

### Approach 2: Hash Table

In my opinion, the second approach cancels the nested loop which should be much faster than the first approach. However, it has no effect at all. :worried:

```python
class Solution:
    def numJewelsInStones(self, J: str, S: str) -> int:
        Hashmap=dict()
        num=0
        # 建立Hash Table
        for i in range(0, len(J)):
            Hashmap[J[i]] = i #键是数值，值是序号
        for j in S:
            if j in Hashmap:
                num+=1
        return num
```

- Runtime: 40 ms, faster than 68.64% of Python3 online submissions for Jewels and Stones.
- Memory Usage: 13 MB, less than 5.25% of Python3 online submissions for Jewels and Stones.