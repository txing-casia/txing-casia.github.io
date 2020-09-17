---
layout:     post
title:      "Q1470 Shuffle the Array-简单"
subtitle:   ""
date:       2020-09-17
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
---

# [Shuffle the Array](https://leetcode-cn.com/problems/shuffle-the-array/)

## Question

> Given the array nums consisting of 2n elements in the form [x1,x2,...,xn,y1,y2,...,yn].
>
> Return the array in the form [x1,y1,x2,y2,...,xn,yn].
>
> **Constraints:**
>
> - `1 <= n <= 500`
> - `nums.length == 2n`
> - `1 <= nums[i] <= 10^3`

> **Example 1:**
>
> Input: nums = [2,5,1,3,4,7], n = 3
>Output: [2,3,5,4,1,7] 
> Explanation: Since x1=2, x2=5, x3=1, y1=3, y2=4, y3=7 then the answer is [2,3,5,4,1,7].

> **Example 2:**
>
> Input: nums = [1,2,3,4,4,3,2,1], n = 4
>Output: [1,4,2,3,3,2,4,1]

> **Example 3:**
>
> Input: nums = [1,1,2,2], n = 2
> Output: [1,2,1,2]

### Approach 1: 循环
本来打算使用单行FOR循环漂亮地完成这道题目：

```python
return [[nums[i],nums[n+i]] for i in range(n)]
```

不幸的是这个写法导致结果变成了一个嵌套的两层的列表，为了消除嵌套的括号，尝试了利用zip(*)解压，.replace("[",'')等操作

- zip(a,b)是合并/压缩，zip(*c)是拆分/解压，并返回一个地址，使用\* zip()和\* zip(\*)则返回数据。如果想连续解压两次写成嵌套形式，就有点问题，比如解压a，写为：\*zip(\*\*zip(\*a))会造成混乱。因此不能实现效果。
- replace则会多出引号

没办法，常规写法：

```python
class Solution(object):
    def shuffle(self, nums, n):
        ans=nums[:]
        j=0
        for i in range(n):
            ans[j],ans[j+1]=nums[i],nums[n+i]
            j=j+2
        return ans


def main():
    nums =[2, 5, 1, 3, 4, 7]
    n = 3
    solution=Solution().shuffle(nums,n)
    print(solution)


if __name__ == "__main__":
    main()
```

**注意：**列表**赋值的时候是地址传递**，例如ans=nums，修改ans会改变nums

### 复杂度分析

- 时间复杂度：O(N)

- 空间复杂度：O(N)




