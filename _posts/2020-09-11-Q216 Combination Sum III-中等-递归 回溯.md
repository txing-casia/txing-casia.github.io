---
layout:     post
title:      "Q216 Combination Sum III-中等-递归 回溯"
subtitle:   ""
date:       2020-09-11
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Medium
    - Backtracking
    - Recursion
---

# [Combination Sum III](https://leetcode-cn.com/problems/combination-sum-iii/)

## Question

> Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.
>
> **Note:**
>
> - All numbers will be positive integers.
> - The solution set must not contain duplicate combinations.

> **Example 1:**
>
> Input: k = 3, n = 7
> Output: [[1,2,4]]

> **Example 2:**
>
> Input: k = 3, n = 9
> Output: [[1,2,6], [1,3,5], [2,3,4]]

### Approach 1: 递归回溯
做了Combination Sum I-III三道题后，对回溯法寻找合适解的问题逐渐熟悉了起来。dfs函数里面需要用if判断所得解是否合适，合适了就使用append方法，把暂存器temp到答案ans里面并return结束此分支的迭代。如果不合适，即未满足终止条件，继续搜索过程。

这里需要注意的是，如果搜索范围有改变（存在不选重复数字之类的条件），记得用一个for循环在新的范围内执行dfs。如果搜索范围没有改变，就可以不用for循环，减小复杂度。

```python
class Solution(object):
    def combinationSum3(self, k: int, n: int):
        def dfs(temp,S,k,n):
            if len(temp)==k and n==0:
                ans.append(temp[:])
                return
            for i in range(len(S)):
                temp.append(S[i])
                # S[i + 1:]实现了剪枝和防止逆序选数，这是关键点
                dfs(temp, S[i+1:], k, n-S[i])
                temp.pop()
        ans=[]
        S=[i for i in range(1,10)]
        # 暂存数组，可选项，需要k个数，需要和为n
        dfs([], S, k, n)
        return ans

def main():
    k = 3
    n = 9
    solution=Solution().combinationSum3(k,n)
    print(solution)

if __name__ == "__main__":
    main()
```



