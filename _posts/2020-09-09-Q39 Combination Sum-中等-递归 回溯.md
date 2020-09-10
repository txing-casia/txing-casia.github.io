---
layout:     post
title:      "Q39 Combination Sum-中等-递归 回溯"
subtitle:   ""
date:       2020-09-09
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Medium
    - Backtracking
    - Recursion
---

# [Combination Sum](https://leetcode-cn.com/problems/combination-sum/)

## Question

> Given a set of candidate numbers (candidates) (without duplicates) and a target number (target), find all unique combinations in candidates where the candidate numbers sums to target.
>
> The same repeated number may be chosen from candidates unlimited number of times.
>
> **Note:**
>
> - All numbers (including target) will be positive integers.
> - The solution set must not contain duplicate combinations.
>
> 

> **Example 1:**
>
> Input: candidates = [2,3,6,7], target = 7,
>A solution set is:
> [
>      [7],
>      [2,2,3]
>    ]
>    

> **Example 2:**
>
> Input: candidates = [2,3,5], target = 8,
> A solution set is:
> [
>   [2,2,2,2],
>   [2,3,3],
>   [3,5]
> ]

> **Constraints:**
>
> 1 <= candidates.length <= 30
> 1 <= candidates[i] <= 200
> Each element of candidate is unique.
> 1 <= target <= 500

### Approach 1: 搜索回溯
#### 思路与算法

对于这类寻找所有可行解的题，我们都可以尝试用「搜索回溯」的方法来解决。

回到本题，我们定义递归函数 dfs(target, combine, idx) 表示当前在 candidates 数组的第 idx 位，还剩 target 要组合，已经组合的列表为 combine。递归的终止条件为 target <= 0 或者 candidates 数组被全部用完。那么在当前的函数中，每次我们可以选择跳过不用第 idx 个数，即执行 dfs(target, combine, idx + 1)。也可以选择使用第 idx 个数，即执行 dfs(target - candidates[idx], combine, idx)，注意到每个数字可以被无限制重复选取，因此搜索的下标仍为 idx。

更形象化地说，如果我们将整个搜索过程用一个树来表达，即如下图呈现，每次的搜索都会延伸出两个分叉，直到递归的终止条件，这样我们就能不重复且不遗漏地找到所有可行解：

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        ans=[] # 汇总可行解
        temp=[] # 存放当前解
        def recursion(idx,sum):
            if idx >= len(candidates) or sum >= target: # 判断合法，不合法则检测是否满足target；合法则继续迭代
                if sum - target == 0:
                    ans.append(temp[:])
                return
            temp.append(candidates[idx])
            recursion(idx, sum + candidates[idx])
            temp.pop() # 删除最后一个元素
            recursion(idx + 1, sum)
        recursion(0,0)
        return ans
    
def main():
    candidates = [2, 3, 6, 7]
    target = 7
    solution=Solution().combinationSum(candidates,target)
    print(solution)

if __name__ == "__main__":
    main()

```

### 复杂度分析

- 时间复杂度：$$O(S)$$，其中 S 为所有可行解的长度之和。从分析给出的搜索树我们可以看出时间复杂度取决于搜索树所有叶子节点的深度之和，即所有可行解的长度之和。在这题中，我们很难给出一个比较紧的上界，我们知道 $$O(n \times 2^n)$$ 是一个比较松的上界，即在这份代码中，n 个位置每次考虑选或者不选，如果符合条件，就加入答案的时间代价。但是实际运行的时候，因为不可能所有的解都满足条件，递归的时候我们还会用 $$target - candidates[idx] >= 0$$ 进行剪枝，所以实际运行情况是远远小于这个上界的。
- 空间复杂度：$$O(\textit{target})$$。除答案数组外，空间复杂度取决于递归的栈深度，在最差情况下需要递归 $$O(\textit{target})$$ 层。



