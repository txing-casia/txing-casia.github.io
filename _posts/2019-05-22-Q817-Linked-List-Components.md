---
layout:     post
title:      "Q817 Linked List Components"
subtitle:   ""
date:       2019-05-22
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Linked List
    - Easy
---

# [Linked List Components](<https://leetcode.com/problems/linked-list-components/>)

## Question (It is a wrong problem description!)

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
    def numComponents(self, head: ListNode, G):
        Gset = set(G)
        cur = head
        ans = 0
        while cur:
            if (cur.val in Gset and getattr(cur.next, 'val', None) not in Gset):
                ans += 1
            cur = cur.next

        return ans



def main():
    head=stringToListNode([0,1,2,3,4])# 将输入的元组转换为链表
    G=[0,3,1,4]
    print(Solution().numComponents(head,G))


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: $$O(N + G\text{.length})$$, where *N* is the length of the linked list with root node `head`.

- Space Complexity: $$O(G\text{.length})$$, to store `Gset`.

  


