---
layout:     post
title:      "Q11 Container With Most Water"
subtitle:   ""
date:       2019-04-23
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
---

# [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## Question

> Given *n* non-negative integers $a_1, a_2, ..., a_n$ , where each represents a point at coordinate ($i$, $a_i$).  $n$ vertical lines are drawn such that the two endpoints of line $i$ is at ($i$, $a_i$) and ($i$, $0$). Find two lines, which together with x-axis forms a container, such that the container contains the most water.
>
> ![question_11.jpg](https://raw.githubusercontent.com/txing-casia/txing-casia.github.io/master/img/question_11.jpg)
>
> The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

> Example: 
>
> Input: [1,8,6,2,5,4,8,3,7]
> Output: 49

## Solution 

### Approach 1: Brute Force

In this case, we will simply consider the area for every possible pair of the lines and find out the maximum area out of those.

```python
class Solution:
    def maxArea(self, height) -> int:
        maxarea=0
        for i in range(0,len(height)):
            for j in range(i+1,len(height)):
                maxarea=max(maxarea,min(height[i],height[j])*(j-i))
        return maxarea

if __name__ == "__main__":
    height = [1,8,6,2,5,4,8,3,7]
    answer=Solution()
    results=answer.maxArea(height)
    print(results)
   
```

- Time complexity : $O(n^2)$. Calculating area for all $\frac{n(n-1)}{2}$ height pairs.
- Space complexity : $ O(1)$ Constant extra space is used. 

---

### Approach 2: Two Pointer Approach

The intuition behind this approach is that the area formed between the lines will always be limited by the height of the shorter line. Further, the farther the lines, the more will be the area obtained.

We take two pointers, one at the beginning and one at the end of the array constituting the length of the lines. Futher, we maintain a variable \text{maxarea}maxarea to store the maximum area obtained till now. At every step, we find out the area formed between them, update \text{maxarea}maxarea and move the pointer pointing to the shorter line towards the other end by one step.

How this approach works?

Initially we consider the area constituting the exterior most lines. Now, to maximize the area, we need to consider the area between the lines of larger lengths. If we try to move the pointer at the longer line inwards, we won't gain any increase in area, since it is limited by the shorter line. But moving the shorter line's pointer could turn out to be beneficial, as per the same argument, despite the reduction in the width. This is done since a relatively longer line obtained by moving the shorter line's pointer might overcome the reduction in area caused by the width reduction.

For further clarification click [here](https://leetcode.com/problems/container-with-most-water/discuss/6099/yet-another-way-to-see-what-happens-in-the-on-algorithm) and for the proof click [here](https://leetcode.com/problems/container-with-most-water/discuss/6089/Anyone-who-has-a-O(N)-algorithm/7268).

```python

class Solution:
    def maxArea(self, height) -> int:
        maxarea=0
        x=len(height)-1 # 最远端的指针
        y=0 # 最近端的指针
        while(y<x):
            maxarea = max(maxarea, min(height[x], height[y]) * (x - y))
            if (height[y] < height[x]):
                y=y+1
            else:
                x=x-1
        return maxarea

if __name__ == "__main__":
    height = [1,8,6,2,5,4,8,3,7]
    answer=Solution()
    results=answer.maxArea(height)
    print(results)
```

**Complexity Analysis**

- Time complexity : $O(n)$. Single pass.
- Space complexity : $O(1)$. Constant space is used.