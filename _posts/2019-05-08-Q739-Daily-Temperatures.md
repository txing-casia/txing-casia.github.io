---
layout:     post
title:      "Q739 Daily Temperatures"
subtitle:   "Stack"
date:       2019-05-08
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Stack
---

# [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)

## Question

> Given a list of daily temperatures `T`, return a list such that, for each day in the input, tells you how many days you would have to wait until a warmer temperature. If there is no future day for which this is possible, put `0` instead.
>
> For example, given the list of temperatures `T = [73, 74, 75, 71, 69, 72, 76, 73]`, your output should be `[1, 1, 4, 2, 1, 1, 0, 0]`.

> **Note**:
>
> `The length of `temperatures` will be in the range `[1, 30000]`. Each temperature will be an integer in the range `[30, 100]`.



### Approach 1: Stack

**Intuition**

Consider trying to find the next warmer occurrence at `T[i]`. What information (about `T[j]` for `j > i`) must we remember?

Say we are trying to find `T[0]`. If we remembered `T[10] = 50`, knowing `T[20] = 50` wouldn't help us, as any `T[i]` that has its next warmer ocurrence at `T[20]` would have it at `T[10]` instead. However, `T[20] = 100` would help us, since if `T[0]` were `80`, then `T[20]` might be its next warmest occurrence, while `T[10]`couldn't.

Thus, we should remember a list of indices representing a strictly increasing list of temperatures. For example, `[10, 20, 30]` corresponding to temperatures `[50, 80, 100]`. When we get a new temperature like `T[i] = 90`, we will have `[5, 30]` as our list of indices (corresponding to temperatures `[90, 100]`). The most basic structure that will satisfy our requirements is a *stack*, where the top of the stack is the first value in the list, and so on.

**Algorithm**

As in *Approach #1*, process indices `i` in descending order. We'll keep a `stack` of indices such that `T[stack[-1]] < T[stack[-2]] < ...`, where `stack[-1]` is the top of the stack, `stack[-2]` is second from the top, and so on; and where `stack[-1] > stack[-2] > ...`; and we will maintain this invariant as we process each temperature.

After, it is easy to know the next occurrence of a warmer temperature: it's simply the top index in the stack.

Here is a worked example of the contents of the `stack` as we work through `T = [73, 74, 75, 71, 69, 72, 76, 73]` in reverse order, at the end of the loop (after we add `T[i]`). For clarity, `stack` only contains indices `i`, but we will write the value of `T[i]` beside it in brackets, such as `0 (73)`.

- When `i = 7`, `stack = [7 (73)]`. `ans[i] = 0`.
- When `i = 6`, `stack = [6 (76)]`. `ans[i] = 0`.
- When `i = 5`, `stack = [5 (72), 6 (76)]`. `ans[i] = 1`.
- When `i = 4`, `stack = [4 (69), 5 (72), 6 (76)]`. `ans[i] = 1`.
- When `i = 3`, `stack = [3 (71), 5 (72), 6 (76)]`. `ans[i] = 2`.
- When `i = 2`, `stack = [2 (75), 6 (76)]`. `ans[i] = 4`.
- When `i = 1`, `stack = [1 (74), 2 (75), 6 (76)]`. `ans[i] = 1`.
- When `i = 0`, `stack = [0 (73), 1 (74), 2 (75), 6 (76)]`. `ans[i] = 1`.

```python
class Solution:
    def dailyTemperatures(self, T):
        ans = [0] * len(T)
        stack = [] #从大到小的序号
        for i in range(len(T)-1, -1, -1):# 从最后一个元素开始
            while stack and T[i] >= T[stack[-1]]:
                stack.pop()# 移除最后一个元素
            if stack:
                ans[i] = stack[-1] - i
            stack.append(i)
        return ans



def main():
    A =[73, 74, 75, 71, 69, 72, 76, 73]# 73, 74, 75, 71, 69, 72, 76, 73
    answer=Solution()
    results=answer.dailyTemperatures(A)
    print(results) # [1, 1, 4, 2, 1, 1, 0, 0]


if __name__ == "__main__":
    main()
```

- Runtime: 308 ms, faster than 70.55% of Python3 online submissions for Daily Temperatures.

- Memory Usage: 16.7 MB, less than 9.45% of Python3 online submissions for Daily Temperatures.

  Next challenges: