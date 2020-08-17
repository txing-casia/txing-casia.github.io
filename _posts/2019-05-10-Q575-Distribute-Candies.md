---
layout:     post
title:      "Q575 Distribute Candies"
subtitle:   "Using hash table or set to find the number of kinds"
date:       2019-05-10
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Hash Table
---

# [Distribute Candies](<https://leetcode.com/problems/distribute-candies/>)

## Question

> Given an integer array with **even** length, where different numbers in this array represent different **kinds** of candies. Each number means one candy of the corresponding kind. You need to distribute these candies **equally** in number to brother and sister. Return the maximum number of **kinds** of candies the sister could gain.

> **Example 1:**
>
> Input: candies = [1,1,2,2,3,3]
> Output: 3
> Explanation:
> There are three different kinds of candies (1, 2 and 3), and two candies for each kind.
> Optimal distribution: The sister has candies [1,2,3] and the brother has candies [1,2,3], too. 
> The sister has three different kinds of candies. 

> **Example 2:**
>
> Input: candies = [1,1,2,3]
> Output: 2
> Explanation: For example, the sister has candies [2,3] and the brother has candies [1,1]. 
> The sister has two different kinds of candies, the brother has only one kind of candies. 

> **Note:**
>
> 1. The length of the given array is in range [2, 10,000], and will be even.
> 2. The number in given array is in range [-100,000, 100,000].

### Approach 1: Hash Table

**Intuition and Algorithm**

First, counting the number of kinds. Then, returning the number of given kinds.

```python
class Solution:
    def distributeCandies(self, candies):
        Hash=dict()
        for i in range(0,len(candies)):
            Hash[candies[i]]=i
        kinds=len(Hash)

        return min(kinds,len(candies)//2)


def main():
    candies =[1,1,1,1,2,2,2,3,3,3]

    answer=Solution()
    results=answer.distributeCandies(candies)
    print(results)


if __name__ == "__main__":
    main()
```

- Runtime: 136 ms, faster than 35.19% of Python3 online submissions for Distribute Candies.
- Memory Usage: 14.5 MB, less than 5.88% of Python3 online submissions for Distribute Candies.

---

### Approach 2: Using Set

**Intuition and Algorithm**

Another way to find the number of unique elements is to traverse over all the elements of the given candies array and keep on putting the elements in a set. By the property of a set, it will contain only unique elements. At the end, we can count the number of elements in the set, given by, say count. The value to be returned will again be given by $$\text{min}(count, n/2)$$. Here,  $$n$$ refers to the size of the candies array.

```python
class Solution:
    def distributeCandies(self, candies):
        candyType = set(candies)# 统计糖果种类数
        return min(len(candyType), len(candies) //2)
    # 如果种类数>n/2，则最多给n/2种糖果
    # 如果种类数<n/2，则给所有种类的糖果
```

- Runtime: 104 ms, faster than ***99.13%*** of Python3 online submissions for Distribute Candies.

- Memory Usage: 14.8 MB, less than 5.88% of Python3 online submissions for Distribute Candies.



