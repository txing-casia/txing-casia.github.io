---
layout:     post
title:      "Q1431 Kids With the Greatest Number of Candies-简单-枚举"
subtitle:   ""
date:       2020-09-15
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - Enumeration
---

# [Kids With the Greatest Number of Candies](https://leetcode-cn.com/problems/kids-with-the-greatest-number-of-candies/)

## Question

> Given the array candies and the integer extraCandies, where candies[i] represents the number of candies that the ith kid has.
>
> For each kid check if there is a way to distribute extraCandies among the kids such that he or she can have the greatest number of candies among them. Notice that multiple kids can have the greatest number of candies.
>
> **Constraints:**
>
> 2 <= candies.length <= 100
> 1 <= candies[i] <= 100
> 1 <= extraCandies <= 50

> **Example 1:**
>
> Input: candies = [2,3,5,1,3], extraCandies = 3
> Output: [true,true,true,false,true] 
> Explanation: 
> Kid 1 has 2 candies and if he or she receives all extra candies (3) will have 5 candies --- the greatest number of candies among the kids. 
> Kid 2 has 3 candies and if he or she receives at least 2 extra candies will have the greatest number of candies among the kids. 
> Kid 3 has 5 candies and this is already the greatest number of candies among the kids. 
> Kid 4 has 1 candy and even if he or she receives all extra candies will only have 4 candies. 
> Kid 5 has 3 candies and if he or she receives at least 2 extra candies will have the greatest number of candies among the kids. 

> **Example 2:**
>
> Input: candies = [4,2,1,1,2], extraCandies = 1
> Output: [true,false,false,false,false] 
> Explanation: There is only 1 extra candy, therefore only kid 1 will have the greatest number of candies among the kids regardless of who takes the extra candy.

> **Example 3:**
>
> Input: candies = [12,1,12], extraCandies = 10
> Output: [true,false,true]

### Approach 1: 枚举法
这道题目关键点在于判断*candies[i] + extraCandies >= max_candies* ，因此需要先找出最大的糖果数，之后在一个for循环中诸葛对比。

```python
class Solution(object):
    def kidsWithCandies(self, candies , extraCandies ) :
        sort_candies=sorted(candies)
        ans=list()
        for i in range(len(candies)):
            if candies[i]+extraCandies>=sort_candies[-1]:
                ans.append(bool(1))
            else:
                ans.append(bool(0))
        return ans


def main():
    candies = [2, 3, 5, 1, 3]
    extraCandies = 3
    solution=Solution().kidsWithCandies(candies,extraCandies)
    print(solution)


if __name__ == "__main__":
    main()
```

上面的代码中使用了sorted排序，其实没有必要，因为只需求到最大值。for循环也可以使用简洁形式表达，使代码更加紧凑

```python
class Solution:
    def kidsWithCandies(self, candies , extraCandies ) :
        maxCandies = max(candies)
        ret = [candy + extraCandies >= maxCandies for candy in candies]
        return ret
```

### 复杂度分析

- 时间复杂度：我们首先使用 O(n)的时间预处理出所有小朋友拥有的糖果数目最大值。对于每一个小朋友，我们需要 O(1) 的时间判断这个小朋友是否可以拥有最多的糖果，故渐进时间复杂度为 O(n)。

- 空间复杂度：这里只用了常数个变量作为辅助空间，与 n 的规模无关，故渐进空间复杂度为 O(1)。

  



