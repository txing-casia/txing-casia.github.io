---
layout:     post
title:      "Q83 Remove Duplicates from Sorted List"
subtitle:   ""
date:       2019-05-28
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Linked List
    - Easy
---

# [Remove Duplicates from Sorted List](<https://leetcode.com/problems/remove-duplicates-from-sorted-list/>)

## Question 

> Given a sorted linked list, delete all duplicates such that each element appear only *once*.

> **Example 1:**
> Input: 1->1->2
> Output: 1->2

> **Example 1:**
> Input: 1->1->2->3->3
> Output: 1->2->3

### Approach 1: Dummy List

This problem is similar to Q21, because they both require traverse a linked list. In fact, the key point is to set  `out=mid=head`. `out` is used in the iterations and `mid` is used as an output.

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
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        out=mid=head
        while head:
            if out.val!=head.val:
                out.next=head
                out=out.next
            head=head.next
            out.next = None

        return mid


def main():
    head=stringToListNode([1,1,2,3,3])# 将输入的元组转换为链表
    solution=Solution().deleteDuplicates(head)
    print(listNodeToString(solution))


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Runtime: 48 ms, faster than 87.51% of Python3 online submissions forRemove Duplicates from Sorted List.
- Memory Usage: 13.3 MB, less than 22.63% of Python3 online submissions for Remove Duplicates from Sorted List.

### Approach 2: Straight-Forward

This is a simple problem that merely tests your ability to manipulate list node pointers. Because the input list is sorted, we can determine if a node is a duplicate by comparing its value to the node *after* it in the list. If it is a duplicate, we change the `next` pointer of the current node so that it skips the next node and points directly to the one after the next node.

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        out = head
        while out and out.next:
            if out.val==out.next.val:
                out.next = out.next.next
            else:
                out=out.next
        return head
```

**Complexity Analysis**

- Time complexity : *O*(*n*). Because each node in the list is checked exactly once to determine if it is a duplicate or not, the total run time is *O*(*n*), where *n* is the number of nodes in the list.

- Space complexity : *O*(1). No additional space is used.

  

- Runtime: 48 ms, faster than 87.51% of Python3 online submissions forRemove Duplicates from Sorted List.
- Memory Usage: 13.3 MB, less than 22.63% of Python3 online submissions for Remove Duplicates from Sorted List.