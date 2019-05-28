---
layout:     post
title:      "Q237 Delete Node in a Linked List"
subtitle:   ""
date:       2019-05-24
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Linked List
    - Easy
---

# [Delete Node in a Linked List](<https://leetcode.com/problems/delete-node-in-a-linked-list/>)

## Question 

> We are given `head`, the head node of a linked list containing **unique integer values**.
>
> We are also given the list `G`, a subset of the values in the linked list.
>
> Return the number of connected components in `head`, where two values are connected if they appear consecutively in the linked list.

> **Example 1:**
> Input: 
> head: 0->1->2->3
> G = [0, 1, 3]
> Output: 2
> Explanation: 
> 0 and 1 are connected, so [0, 1] and [3] are the two connected components.

> **Example 2:**
> Input: 
> head: 0->1->2->3->4
> G = [0, 3, 1, 4]
> Output: 2
> Explanation: 
> 0 and 1 are connected, 3 and 4 are connected, so [0, 1] and [3, 4] are the two connected components.

### Approach 1: Grouping

> Note: There is something wrong in the problem description. The test cases online are confusing.
>
> **Example:**
>
> input: [0] and [0]
>
> expected output: [1]



**Intuition**

Instead of thinking about connected components in `G`, think about them in the linked list. Connected components in `G` must occur consecutively in the linked list.

**Algorithm**

Scanning through the list, if `node.val` is in `G` and `node.next.val` isn't (including if `node.next` is `null`), then this must be the end of a connected component.

For example, if the list is `0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7`, and `G = [0, 2, 3, 5, 7]`, then when scanning through the list, we fulfill the above condition at `0, 3, 5, 7`, for a total answer of `4`.

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


class Solution:
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        if node.next is not None:
            node.val = node.next.val
            node.next = node.next.next


def main():
    head=stringToListNode([4,5,1,9])# 将输入的元组转换为链表
    dele=head.next.next
    Solution().deleteNode(dele)
    print(listNodeToString(head))


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time and space complexity are both *O*(1).

- Runtime: 36 ms, faster than 99.70% of Python3 online submissions forDelete Node in a Linked List.

- Memory Usage: 13.5 MB, less than 55.73% of Python3 online submissions for Delete Node in a Linked List.