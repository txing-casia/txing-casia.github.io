---
layout:     post
title:      "Q77 Combinations-递归 回溯"
subtitle:   ""
date:       2020-09-08
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Medium
    - Backtracking Algorithm
---

# [Combinations](https://leetcode-cn.com/problems/combinations/)

## Question

> Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.
>
> You may return the answer in any order.
>
> **Constraints:**
>
> - `1 <= n <= 20`
> - `1 <= k <= n`

> **Example 1:**
>
> Input: n = 4, k = 2
>Output:
> [
>      [2,4],
>      [3,4],
>     [2,3],
>      [1,2],
>      [1,3],
>   [1,4],
>]

> **Example 2:**
>
> Input: n = 1, k = 1
> Output: [[1]]

### Approach 1: 递归调用

把选k个数的任务拆解为选2个数的任务，直到选完了k个数

```python
class Solution(object):
    def combine(self, n, k):
        if k>n or k==0:
            return []
        if k==1:
            return [[i] for i in range(1,n+1)]
        if k==n:
            return [[i for i in range(1,n+1)]]
        
        answer=self.combine(n-1,k)
        for item in self.combine(n-1,k-1):
            item.append(n)
            answer.append(item)
        
        return answer
    
def main():
    n = 4
    k = 2
    solution=Solution().combine(n,k)
    print(solution)


if __name__ == "__main__":
    main()

```



### Approach 2: [回溯算法](https://leetcode-cn.com/problems/combinations/solution/hui-su-suan-fa-jian-zhi-python-dai-ma-java-dai-ma-/)

#### 重点概括：

- 如果解决一个问题有多个步骤，每一个步骤有多种方法，题目又要我们找出所有的方法，可以使用回溯算法；

- 回溯算法是在一棵树上的 深度优先遍历（因为要找所有的解，所以需要遍历）；
- 组合问题，相对于排列问题而言，不计较一个组合内元素的顺序性（即 [1, 2, 3] 与 [1, 3, 2] 认为是同一个组合），因此很多时候需要按某种顺序展开搜索，这样才能做到不重不漏。

既然是树形问题上的 深度优先遍历，因此首先画出树形结构。例如输入：n = 4, k = 2，我们可以发现如下递归结构：

- 如果组合里有 1 ，那么需要在 [2, 3, 4] 里再找 11 个数；
- 如果组合里有 2 ，那么需要在 [3, 4] 里再找 11数。注意：这里不能再考虑 11，因为包含 11 的组合，在第 1 种情况中已经包含。
依次类推（后面部分省略），以上描述体现的 递归 结构是：在以 nn 结尾的候选数组里，选出若干个元素。画出递归结构如下图：

#### 说明：

叶子结点的信息体现在从根结点到叶子结点的路径上，因此需要一个表示路径的变量 path，它是一个列表，特别地，path 是一个栈；
每一个结点递归地在做同样的事情，区别在于搜索起点，因此需要一个变量 start ，表示在区间 [begin, n] 里选出若干个数的组合；
可能有一些分支没有必要执行，我们放在优化中介绍。
友情提示：对于这一类问题，画图帮助分析是非常重要的解题方法。

```python
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        if (k <= 0 || n < k) {
            return res;
        }
        // 从 1 开始是题目的设定
        Deque<Integer> path = new ArrayDeque<>();
        dfs(n, k, 1, path, res);
        return res;
    }

    private void dfs(int n, int k, int begin, Deque<Integer> path, List<List<Integer>> res) {
        // 递归终止条件是：path 的长度等于 k
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }

        // 遍历可能的搜索起点
        for (int i = begin; i <= n; i++) {
            // 向路径变量里添加一个数
            path.addLast(i);
            // 下一轮搜索，设置的搜索起点要加 1，因为组合数理不允许出现重复的元素
            dfs(n, k, i + 1, path, res);
            // 重点理解这里：深度优先遍历有回头的过程，因此递归之前做了什么，递归之后需要做相同操作的逆向操作
            path.removeLast();
        }
    }
}

```

#### 优化

事实上，如果 n = 7, k = 4，从 55 开始搜索就已经没有意义了，这是因为：即使把 55 选上，后面的数只有 66 和 77，一共就 33 个候选数，凑不出 44 个数的组合。因此，搜索起点有上界，这个上界是多少，可以举几个例子分析。

分析搜索起点的上界，其实是在深度优先遍历的过程中剪枝，剪枝可以避免不必要的遍历，剪枝剪得好，可以大幅度节约算法的执行时间。

下面的图片绿色部分是剪掉的枝叶，当 n 很大的时候，能少遍历很多结点，节约了时间。

（温馨提示：右键，在弹出的下拉列表框中选择「在新标签页中打开图片」，可以查看大图。）

容易知道：搜索起点和当前还需要选几个数有关，而当前还需要选几个数与已经选了几个数有关，即 path 的长度相关。我们举几个例子分析：

例如：n = 6 ，k = 4。

path.size() == 1 的时候，接下来要选择 33 个数，搜索起点最大是 44，最后一个被选的组合是 [4, 5, 6]；
path.size() == 2 的时候，接下来要选择 22 个数，搜索起点最大是 55，最后一个被选的组合是 [5, 6]；
path.size() == 3 的时候，接下来要选择 11 个数，搜索起点最大是 66，最后一个被选的组合是 [6]；

再如：n = 15 ，k = 4。
path.size() == 1 的时候，接下来要选择 33 个数，搜索起点最大是 1313，最后一个被选的是 [13, 14, 15]；
path.size() == 2 的时候，接下来要选择 22 个数，搜索起点最大是 1414，最后一个被选的是 [14, 15]；
path.size() == 3 的时候，接下来要选择 11 个数，搜索起点最大是 1515，最后一个被选的是 [15]；

搜索起点的上界 + 接下来要选择的元素个数 - 1 = n

其中，接下来要选择的元素个数 = k - path.size()，整理得到：


搜索起点的上界 = n - (k - path.size()) + 1
所以，我们的剪枝过程就是：把 i <= n 改成 i <= n - (k - pre.size()) + 1 ：

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.Deque;
import java.util.List;

public class Solution {

    public List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> res = new ArrayList<>();
        if (k <= 0 || n < k) {
            return res;
        }
        Deque<Integer> path = new ArrayDeque<>();
        dfs(n, k, 1, path, res);
        return res;
    }

    private void dfs(int n, int k, int index, Deque<Integer> path, List<List<Integer>> res) {
        if (path.size() == k) {
            res.add(new ArrayList<>(path));
            return;
        }

        // 只有这里 i <= n - (k - path.size()) + 1 与参考代码 1 不同
        for (int i = index; i <= n - (k - path.size()) + 1; i++) {
            path.addLast(i);
            dfs(n, k, i + 1, path, res);
            path.removeLast();
        }
    }
}
```

#### 说明：

一些边界条件比较绕的，用具体的例子分析就不容易出错，主要考察的是细心，没有太多技巧；
为参考代码 3 添加 path 的打印输出语句，可以看到输出语句会更少。

```python
递归之前 => [1]
递归之前 => [1, 2]
递归之前 => [1, 2, 3]
递归之后 => [1, 2]
递归之前 => [1, 2, 4]
递归之后 => [1, 2]
递归之前 => [1, 2, 5]
递归之后 => [1, 2]
递归之后 => [1]
递归之前 => [1, 3]
递归之前 => [1, 3, 4]
递归之后 => [1, 3]
递归之前 => [1, 3, 5]
递归之后 => [1, 3]
递归之后 => [1]
递归之前 => [1, 4]
递归之前 => [1, 4, 5]
递归之后 => [1, 4]
递归之后 => [1]
递归之后 => []
递归之前 => [2]
递归之前 => [2, 3]
递归之前 => [2, 3, 4]
递归之后 => [2, 3]
递归之前 => [2, 3, 5]
递归之后 => [2, 3]
递归之后 => [2]
递归之前 => [2, 4]
递归之前 => [2, 4, 5]
递归之后 => [2, 4]
递归之后 => [2]
递归之后 => []
递归之前 => [3]
递归之前 => [3, 4]
递归之前 => [3, 4, 5]
递归之后 => [3, 4]
递归之后 => [3]
递归之后 => []
[[1, 2, 3], [1, 2, 4], [1, 2, 5], [1, 3, 4], [1, 3, 5], [1, 4, 5], [2, 3, 4], [2, 3, 5], [2, 4, 5], [3, 4, 5]]

```



