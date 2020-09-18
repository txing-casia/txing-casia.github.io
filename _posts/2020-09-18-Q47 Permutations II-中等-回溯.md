---
layout:     post
title:      "Q47 Permutations II-中等-回溯"
subtitle:   ""
date:       2020-09-18
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Medium
    - Backtracking
---

# [Permutations II](https://leetcode-cn.com/problems/permutations-ii/)

## Question

> Given a collection of numbers that might contain duplicates, return all possible unique permutations.
>

> **Example 1:**
>
> Input: [1,1,2]
> Output:
> [
>   [1,1,2],
>   [1,2,1],
>   [2,1,1]
> ]

### Approach 1: 循环
本来打算使用Hash table统计一下有哪些元素，以及对应的个数，但后来想了想其实没有必要统计。在写代码的时候找到了一个快速实现字典初始化的方法。

```python
Hash=dict()
for i in range(len(nums)):
    Hash[nums[i]]=0
```

可以用下面这句等效替代，节省一个For循环：

```python
Hash.setdefault(nums[i], 0)
```

下面正片开始

```python

class Solution(object):
    def permuteUnique(self, nums):
        # 错误输入检测
        if not nums:
            return []

        def dfs(nums, temp_list):
            if len(temp_list) == n:
                ans.append(temp_list) # temp_list存满了就放入ans
            for i in range(len(nums)):
                if i > 0 and nums[i] == nums[i-1]: # 第一个元素不进行此判断
                    continue
                dfs(nums[:i]+nums[i+1:], temp_list+[nums[i]])

        ans = []
        n = len(nums)
        nums.sort()
        dfs(nums, [])
        return ans


def main():
    nums =[1,1,2]
    solution=Solution().permuteUnique(nums)
    print(solution)


if __name__ == "__main__":
    main()
```

主要利用排序后对列表的切片操作，巧妙地对排列进行剪支

写的非常简洁，值得好好研究

## 复杂度分析

- 时间复杂度：$$O(n\times n!)$$

  算法的复杂度首先受 backtrack 的调用次数制约，backtrack 的调用次数为 $$\sum_{k = 1}^{n}{P(n, k)}$$次，其中 $$P(n, k) = \frac{n!}{(n - k)!} = n (n - 1) ... (n - k + 1)$$，该式被称作 n 的 k - 排列，或者部分排列

  这说明 backtrack 的调用次数是 $$O(n!)$$的。

  而对于 backtrack 调用的每个叶结点（最坏情况下没有重复数字共 $$n!$$ 个），我们需要将当前答案使用 $$O(n)$$ 的时间复制到答案数组中，相乘得时间复杂度为 $$O(n \times n!)$$。

- 空间复杂度：$$O(n )$$。递归的时候栈深度会达到 O(n)*O*(*n*)，因此总空间复杂度为 O(n + n)=O(2n)=O(n)*O*(*n*+*n*)=*O*(2*n*)=*O*(*n*)。