---
layout:     post
title:      "Q206 Reverse Linked List"
subtitle:   ""
date:       2019-05-21
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Linked List
    - Easy
---

# [Reverse Linked List](<https://leetcode.com/problems/reverse-linked-list/>)

## Question

> Reverse a singly linked list.

> **Example 1:**
> Input: 1->2->3->4->5->NULL
> Output: 5->4->3->2->1->NULL

### Approach 1: Output to Array

**Intuition and Algorithm**



```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None


class Solution:
    def reverseList(self, head):
        prev_node = None
        curr_node = head
        while curr_node:
            next_node = curr_node.next  # Remember next node
            curr_node.next = prev_node  # REVERSE! None, first time round.
            prev_node = curr_node  # Used in the next iteration.
            curr_node = next_node  # Move to next node.
        head = prev_node
        return head



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
    return "[" + result[:-2] + "]"


def main():
    head=stringToListNode([1,2,3,4,5])

    print(listNodeToString(Solution().reverseList(head)))


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Runtime: 28 ms, faster than 99.98% of Python3 online submissions forReverse Linked List.
- Memory Usage: 14.5 MB, less than 50.22% of Python3 online submissions for Reverse Linked List.
