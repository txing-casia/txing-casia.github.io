---
layout:     post
title:      "Q60 Permutation Sequence-中等"
subtitle:   ""
date:       2020-09-05
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Medium
---

# [Permutation Sequence](https://leetcode-cn.com/problems/permutation-sequence/)

## Question

> The set [1,2,3,...,n] contains a total of n! unique permutations.
>
> By listing and labeling all of the permutations in order, we get the following sequence for n = 3:
>
> "123"
> "132"
> "213"
> "231"
> "312"
> "321"
> Given n and k, return the kth permutation sequence.
>
> **Note:**
>
> Given n will be between 1 and 9 inclusive.
> Given k will be between 1 and n! inclusive.

> **Example 1:**
>
> Input: n = 3, k = 3
> Output: "213"

> **Example 2:**
>
> Input: n = 4, k = 9
> Output: "2314"

### Approach 1: 缩小问题规模

要想解决本题，首先需要了解一个简单的结论：

> 对于 n 个不同的元素（例如数 1, 2, $$\cdots$$, n），它们可以组成的排列总数目为 n!。

对于给定的 n 和 k，我们不妨从左往右确定第 k 个排列中的每一个位置上的元素到底是什么。

我们首先确定排列中的首个元素 $$a_1$$ 。根据上述的结论，我们可以知道：

- 以 1 为 $$a_1$$ 的排列一共有 $$(n-1)!$$ 个；

- 以 2 为 $$a_1$$的排列一共有 $$(n-1)!$$ 个；

- 以 n 为 $$a_1$$ 的排列一共有 $$(n-1)!$$ 个。

由于我们需要求出从小到大的第 k 个排列，因此：

- 如果 $$k \leq (n-1)!$$，我们就可以确定排列的首个元素为 1；
- 如果 $$(n-1)! < k \leq 2 \cdot (n-1)!$$，我们就可以确定排列的首个元素为 2；
- 如果 $$(n-1) \cdot (n-1)! < k \leq n \cdot (n-1)!$$，我们就可以确定排列的首个元素为 n。
  因此，第 k 个排列的首个元素就是：

$$
a_1=\frac{k−1}{(n−1)!}+1
$$

当我们确定了 $$a_1$$ 后，如何使用相似的思路，确定下一个元素 $$a_2$$  呢？实际上，我们考虑以 $$a_1$$  为首个元素的所有排列：

- 以 $$[1,n] \backslash a_1$$最小的元素为 $$a_2$$ 的排列一共有 $$(n-2)!$$个；
- 以 $$[1,n] \backslash a_1$$ 次小的元素为 $$a_2$$的排列一共有 $$(n-2)!$$个；
- 以 $$[1,n] \backslash a_1$$最大的元素为 $$a_2$$的排列一共有 $$(n-2)!$$ 个；

其中 $$[1,n] \backslash a_1$$表示包含 $$1, 2, \cdots n$$中除去 $$a_1$$以外元素的集合。这些排列从编号 $$(a_1-1) \cdot (n-1)!$$开始，到 $$a_1 \cdot (n-1)!$$结束，总计 $$(n-1)!$$个，因此第 k 个排列实际上就对应着这其中的第
$$
k' = (k-1) \bmod (n-1)! + 1
$$
个排列。这样一来，我们就把原问题转化成了一个完全相同但规模减少 1 的子问题：

求 $$[1, n] \backslash a_1$$这 n-1 个元素组成的排列中，第 $$k'$$ 小的排列。

#### 算法

设第 k 个排列为 $$a_1, a_2, \cdots, a_n$$，我们从左往右地确定每一个元素 $$a_i$$。我们用数组 $$\textit{valid}$$记录每一个元素是否被使用过。

我们从小到大枚举 i：

- 我们已经使用过了 i-1 个元素，剩余 n-i+1 个元素未使用过，每一个元素作为 a_i 都对应着 (n-i)!个排列，总计 (n-i+1)! 个排列；

- 因此在第 k 个排列中，$$a_i$$  即为剩余未使用过的元素中第 $$\lfloor \frac{k-1}{(n-i)!}\rfloor + 1$$小的元素；

- 在确定了 $$a_i$$后，这 $$n-i+1$$ 个元素的第 k 个排列，就等于 $$a_i$$之后跟着剩余 n-i 个元素的第 $$(k-1) \bmod (n-i)! + 1$$个排列。

在实际的代码中，我们可以首先将 k 的值减少 1，这样可以减少运算，降低代码出错的概率。对应上述的后两步，即为：

- 因此在第 k 个排列中，$$a_i$$即为剩余未使用过的元素中第 $$\lfloor \frac{k}{(n-i)!} \rfloor + 1$$小的元素；

- 在确定了 $$a_i$$ 后，这 n-i+1 个元素的第 k 个排列，就等于 $$a_i$$  之后跟着剩余 n-i个元素的第 $$k \bmod (n-i)!$$个排列。

实际上，这相当于我们将所有的排列从 0 开始进行编号。

```python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        factorial = [1]
        for i in range(1, n):
            factorial.append(factorial[-1] * i)
        
        k -= 1
        ans = list()
        valid = [1] * (n + 1)
        for i in range(1, n + 1):
            order = k // factorial[n - i] + 1
            for j in range(1, n + 1):
                order -= valid[j]
                if order == 0:
                    ans.append(str(j))
                    valid[j] = 0
                    break
            k %= factorial[n - i]

        return "".join(ans)
```



### 复杂度分析

- 时间复杂度：$$O(n^2)$$。

- 空间复杂度：$$O(n)$$。

  

### 思考题

对于给定的排列 $$a_1, a_2, \cdots, a_n$$ ，你能求出 k 吗？

解答：
$$
k = \left( \sum_{i=1}^n \textit{order}(a_i) \cdot (n-i)! \right) + 1
$$
其中 $$\textit{order}(a_i)$$表示 $$a_{i+1}, \cdots, a_n$$中小于 $$a_i$$的元素个数。

### 反思

我在分析这个问题的时候只考虑到了第一项怎么确定，然后就用递归来求解问题，中间忽略了n和k的取值特例。分析不够完备，代码看着比较乱，可读性不强。今后分析的时候还是要把问题分析完整了再编代码，比如推出这道题的通项公式后再写代码。