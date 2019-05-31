---
layout:     post
title:      "Q1019 Next Greater Node In Linked List"
subtitle:   ""
date:       2019-05-30
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Linked List
    - Medium
    - Stack
---

# [Next Greater Node In Linked List](<https://leetcode.com/problems/next-greater-node-in-linked-list/>)

## Question 

> We are given a linked list with `head` as the first node.  Let's number the nodes in the list: `node_1, node_2, node_3, ...` etc.
>
> Each node may have a *next larger* **value**: for `node_i`, `next_larger(node_i)` is the `node_j.val` such that `j > i`, `node_j.val > node_i.val`, and `j` is the smallest possible choice.  If such a `j` does not exist, the next larger value is `0`.
>
> Return an array of integers `answer`, where `answer[i] = next_larger(node_{i+1})`.
>
> Note that in the example **inputs** (not outputs) below, arrays such as `[2,1,5]` represent the serialization of a linked list with a head node value of 2, second node value of 1, and third node value of 5.

> **Example 1:**
> Input: [2,1,5]
> Output: [5,5,0]

> **Example 2:**
> Input: [2,7,4,3,5]
> Output: [7,0,5,5,0]

> **Example 3:**
>
> Input: [1,7,5,1,9,2,5,1]
> Output: [7,9,9,9,0,5,0,0]

### Approach 1: Stack

Save <index, value> pair to the stack.

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


def stringToListNode(input):
    # Now convert that list into linked list
    dummyRoot = ListNode(0)
    ptr = dummyRoot
    for number in input:
        ptr.next = ListNode(number)
        ptr = ptr.next
    ptr = dummyRoot.next
    return ptr

def listNodeToString(node):
    if not node:
        return "[]"
    result = ""
    while node:
        result += str(node.val) + ", "
        node = node.next
    return "[" + result[:-2] + "]" # 最后一个逗号不输出

class Solution(object):
    def nextLargerNodes(self, head):
        res, stack = [], [] # 建立两个元组
        while head:
            while stack and stack[-1][1] < head.val: # 遍历链表每个元素
                res[stack.pop()[0]] = head.val
            stack.append([len(res), head.val])
            res.append(0)
            head = head.next
        return res


def main():
    head=stringToListNode([2,7,4,3,5])# 将输入的元组转换为链表
    solution=Solution().nextLargerNodes(head)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- O(N) Time, O(N) Space

- Runtime: 356 ms, faster than 76.36% of Python online submissions for Next Greater Node In Linked List.

- Memory Usage: 21 MB, less than 17.81% of Python online submissions for Next Greater Node In Linked List.