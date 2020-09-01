---
layout:     post
title:      "Q486 Predict the Winner-递归 动态规划"
subtitle:   ""
date:       2020-09-01
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Medium
    - Dynamic Planning
---

# [Predict the Winner](https://leetcode-cn.com/problems/predict-the-winner/)

## Question

> Given an array of scores that are non-negative integers. Player 1 picks one of the numbers from either end of the array followed by the player 2 and then player 1 and so on. Each time a player picks a number, that number will not be available for the next player. This continues until all the scores have been chosen. The player with the maximum score wins.
>
> Given an array of scores, predict whether player 1 is the winner. You can assume each player plays to maximize his score.
> 

> **Example 1:**
>
> Input: [1, 5, 2]
> Output: False
> Explanation: Initially, player 1 can choose between 1 and 2. 
> If he chooses 2 (or 1), then player 2 can choose from 1 (or 2) and 5. If player 2 chooses 5, then player 1 will be left with 1 (or 2). 
> So, final score of player 1 is 1 + 2 = 3, and player 2 is 5. 
> Hence, player 1 will never be the winner and you need to return False.

> **Example 2：**
>
> Input: [1, 5, 233, 7]
> Output: True
> Explanation: Player 1 first chooses 1. Then player 2 have to choose between 5 and 7. No matter which number player 2 choose, player 1 can choose 233.
> Finally, player 1 has more score (234) than player 2 (12), so you need to return True representing player1 can win.

> **constraints**
>
> 1 <= length of the array <= 20.
>Any scores in the given array are non-negative integers and will not exceed 10,000,000.
> If the scores of both players are equal, then player 1 is still the winner.



### Approach 1: 递归

为了判断哪个玩家可以获胜，需要计算一个总分，为先手得分与后手得分之差。当数组中的所有数字都被拿取时，如果总分大于或等于 0，则先手获胜，反之则后手获胜。

由于每次只能从数组的任意一端拿取数字，因此可以保证数组中剩下的部分一定是连续的。假设数组当前剩下的部分为下标 $$\textit{start}$$ 到下标 $$\textit{end}$$，其中$$ 0 \le \textit{start} \le \textit{end} < \textit{nums}.\text{length}$$。如果 $$\textit{start}=\textit{end}$$，则只剩一个数字，当前玩家只能拿取这个数字。如果 $$\textit{start}<\textit{end}$$，则当前玩家可以选择 $$\textit{nums}[\textit{start}]$$或 $$\textit{nums}[\textit{end}]$$，然后轮到另一个玩家在数组剩下的部分选取数字。这是一个递归的过程。

计算总分时，需要记录当前玩家是先手还是后手，判断当前玩家的得分应该记为正还是负。当数组中剩下的数字多于 1 个时，当前玩家会选择最优的方案，使得自己的分数最大化，因此对两种方案分别计算当前玩家可以得到的分数，其中的最大值为当前玩家最多可以得到的分数。

```python
class Solution(object):
    def PredictTheWinner_solution_1(self, nums):
        def total(start: int, end: int, turn: int) -> int:
            if start == end:
                return nums[start] * turn
            scoreStart = nums[start] * turn + total(start + 1, end, -turn) # case 1
            scoreEnd = nums[end] * turn + total(start, end - 1, -turn)# case 2
            return max(scoreStart * turn, scoreEnd * turn) * turn
        return total(0, len(nums) - 1, 1) >= 0

def main():
    nums = [2,4,1,2,7,8]
    solution=Solution().PredictTheWinner_solution_1(nums)
    print(solution)

if __name__ == "__main__":
    main()
```



- 时间复杂度：$$O(2^n)$$，其中 n 是数组的长度。
- 空间复杂度：$$O(n)$$，其中 n 是数组的长度。空间复杂度取决于递归使用的栈空间。



## Approach 2: 动态规划

方法一使用递归，存在大量重复计算，因此时间复杂度很高。由于存在重复子问题，因此可以使用动态规划降低时间复杂度。

定义二维数组 $$\textit{dp}$$，其行数和列数都等于数组的长度，$$\textit{dp}[i][j]$$表示当数组剩下的部分为下标 i 到下标 j 时，当前玩家与另一个玩家的分数之差的最大值，注意当前玩家不一定是先手。

只有当 $$i \le j$$ 时，数组剩下的部分才有意义，因此当 $$i>j$$ 时，$$\textit{dp}[i][j]=0$$。

当 $$i=j$$ 时，只剩一个数字，当前玩家只能拿取这个数字，因此对于所有 $$0 \le i < \textit{nums}.\text{length}$$，都有 $$\textit{dp}[i][i]=\textit{nums}[i]$$。

当 $$i<j$$ 时，当前玩家可以选择 $$\textit{nums}[i]$$ 或 $$\textit{nums}[j]$$，然后轮到另一个玩家在数组剩下的部分选取数字。在两种方案中，当前玩家会选择最优的方案，使得自己的分数最大化。因此可以得到如下状态转移方程：

$$\textit{dp}[i][j]=\max(\textit{nums}[i] - \textit{dp}[i + 1][j], \textit{nums}[j] - \textit{dp}[i][j - 1])$$

最后判断 $$\textit{dp}[0][\textit{nums}.\text{length}-1]$$ 的值，如果大于或等于 0，则先手得分大于或等于后手得分，因此先手成为赢家，否则后手成为赢家。

```python
def PredictTheWinner_solution_2(self, nums):
	length = len(nums)
    dp = [[0] * length for _ in range(length)]
    for i, num in enumerate(nums):
        dp[i][i] = num
    for i in range(length - 2, -1, -1):
        for j in range(i + 1, length):
        	dp[i][j] = max(nums[i] - dp[i + 1][j], nums[j] - dp[i][j - 1])
    return dp[0][length - 1] >= 0
```

上述代码中使用了二维数组 $$\textit{dp}$$。分析状态转移方程可以看到，$$\textit{dp}[i][j]$$ 的值只和 $$\textit{dp}[i + 1][j]$$ 与 $$\textit{dp}[i][j - 1]$$ 有关，即在计算 $$\textit{dp}$$ 的第 i 行的值时，只需要使用到 $$\textit{dp}$$ 的第 i 行和第 i+1 行的值，因此可以使用一维数组代替二维数组，对空间进行优化。

```python
def PredictTheWinner_solution_2_1(self, nums):
    length = len(nums)
    dp = [0] * length
    for i, num in enumerate(nums):
        dp[i] = num
    for i in range(length - 2, -1, -1):
        for j in range(i + 1, length):
            dp[j] = max(nums[i] - dp[j], nums[j] - dp[j - 1])
    return dp[length - 1] >= 0
```

- 时间复杂度：$$O(n^2)$$，其中 n 是数组的长度。需要计算每个子数组对应的 $$\textit{dp}$$ 的值，共有 $$\frac{n(n+1)}{2} $$个子数组。

- 空间复杂度：$$O(n)$$，其中 n 是数组的长度。空间复杂度取决于额外创建的数组 $$\textit{dp}$$，如果不优化空间，则空间复杂度是 $$O(n^2)$$，使用一维数组优化之后空间复杂度可以降至 $$O(n)$$。



for 循环使用 enumerate：

```python
>>> seq = ['one', 'two', 'three']
>>> for i, element in enumerate(seq):
>>>     print i, element

output:
0 one
1 two
2 three
```



## 拓展练习
读者在做完这道题之后，可以做另一道类似的题：「877. 石子游戏」。和这道题相比，[第 877 题](https://leetcode-cn.com/problems/stone-game/)增加了两个限制条件：

- 数组的长度是偶数；

- 数组的元素之和是奇数，所以没有平局。

对于第 877 题，除了使用这道题的解法以外，能否利用上述两个限制条件进行求解？
