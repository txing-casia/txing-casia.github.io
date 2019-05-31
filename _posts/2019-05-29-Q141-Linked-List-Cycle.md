---
layout:     post
title:      "Q141 Linked List Cycle"
subtitle:   ""
date:       2019-05-29
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Linked List
    - Easy
---

# [Linked List Cycle](<https://leetcode.com/problems/linked-list-cycle/>)

## Question 

> Given a linked list, determine if it has a cycle in it.
>
> To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

> **Example 1:**
> Input: head = [3,2,0,-4], pos = 1
> Output: true
> Explanation: There is a cycle in the linked list, where tail connects to the second node.

> **Example 2:**
> Input: head = [1,2], pos = 0
> Output: true
> Explanation: There is a cycle in the linked list, where tail connects to the first node.

> **Example 3:**
>
> Input: head = [1], pos = -1
> Output: false
> Explanation: There is no cycle in the linked list.

### Approach 1: Hash Table

This problem is similar to Q21, because they both require traverse a linked list. In fact, the key point is to set  `out=mid=head`. `out` is used in the iterations and `mid`**Intuition**

To detect if a list is cyclic, we can check whether a node had been visited before. A natural way is to use a hash table. is used as an output.

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
    def hasCycle(self, head):
        hashmap=dict()
        while head:
            if not(head in hashmap):
                hashmap[head]=1
            else:
                return True
            head=head.next
        return False



def main():
    head=stringToListNode([1])# 将输入的元组转换为链表
    solution=Solution().hasCycle(head)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Runtime: 32 ms, faster than 99.15% of Python online submissions forLinked List Cycle.
- Memory Usage: 19 MB, less than 7.37% of Python online submissions forLinked List Cycle.

