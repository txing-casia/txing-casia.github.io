---
layout:     post
title:      "Q884 Uncommon Words from Two Sentences"
subtitle:   "(Original) Runtime: 36 ms, faster than 92.30% of Python3 codes. Memory Usage: 13.1 MB, less than 75.47% of Python3 codes"
date:       2019-05-17
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Hash Table
    - Easy
---

# [Uncommon Words from Two Sentences](<https://leetcode.com/problems/uncommon-words-from-two-sentences/>)

## Question

> We are given two sentences `A` and `B`.  (A *sentence* is a string of space separated words.  Each word consists only of lowercase letters.)
>
> A word is *uncommon* if it appears exactly once in one of the sentences, and does not appear in the other sentence.
>
> Return a list of all uncommon words. 
>
> You may return the list in any order.

> **Example 1:**
> Input: A = "this apple is sweet", B = "this apple is sour"
> Output: ["sweet","sour"]

> **Example 2:**
> Input: A = "apple apple", B = "banana"
> Output: ["banana"]

> **Note:**
>
> 1. `0 <= A.length <= 200`
> 2. `0 <= B.length <= 200`
> 3. `A` and `B` both contain only spaces and lowercase letters.

### Approach 1: Counting

**Intuition and Algorithm**

Every uncommon word occurs exactly once in total. We can count the number of occurrences of every word, then return ones that occur exactly once.

```python
class Solution:
    def uncommonFromSentences(self, A, B):
        count = {}
        for i in A.split():
            count[i] = count.get(i, 0) + 1
            # .get(i,0)意思是如果i在count里不存在，返回默认为0
        for i in B.split():
            count[i] = count.get(i, 0) + 1

        #Alternatively:
        #count = collections.Counter(A.split())
        #count += collections.Counter(B.split())

        return [word for word in count if count[word] == 1]


def main():
    A = "this apple is sweet"
    B = "this apple is sour"
    answer=Solution()
    results=answer.uncommonFromSentences(A,B)
    print(results)


if __name__ == "__main__":
    main()
```

- Runtime: 32 ms, faster than 99.14% of Python3 online submissions for Uncommon Words from Two Sentences.
- Memory Usage: 13.3 MB, less than 23.76% of Python3 online submissions for Uncommon Words from Two Sentences.

---

### Approach 2: Counting (Original)

**Intuition and Algorithm**

Every uncommon word occurs exactly once in total. We can count the number of occurrences of every word, then return ones that occur exactly once.

```python
class Solution:
    def uncommonFromSentences(self, A: str, B: str) -> List[str]:
        count = {}
        for i in A.split()+B.split():
            count[i] = count.get(i, 0) + 1
        return [i for i in count if count[i] == 1]
```

- Runtime: 36 ms, faster than 92.30% of Python3 online submissions for Uncommon Words from Two Sentences.

- Memory Usage: 13.1 MB, less than 75.47% of Python3 online submissions for Uncommon Words from Two Sentences.