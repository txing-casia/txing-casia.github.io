---
layout:     post
title:      "Q21 Merge Two Sorted Lists"
subtitle:   ""
date:       2019-05-27
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Linked List
    - Easy
---

# [Merge Two Sorted Lists](<https://leetcode.com/problems/merge-two-sorted-lists/>)

## Question 

> Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

> **Example 1:**
> Input: 1->2->4, 1->3->4
> Output: 1->1->2->3->4->4

### Approach 1: dummy list

This is a straightforward method for our problem. Setting two linked list newNode and out with the same memory location. Then use them to note the merged list.

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
    def mergeTwoLists(self, l1: ListNode, l2: ListNode):
        
        newNode=out=ListNode(0) # newNode与out节点为起始位置相同
        
        while l1 and l2: # 直到两个链表都遍历完
            if l1 is None:
                newNode.next = l2
                l2 = l2.next
            elif l2 is None:
                newNode.next = l1
                l1 = l1.next
            elif l1.val<=l2.val:
                newNode.next=l1
                l1 = l1.next
            else:
                newNode.next = l2
                l2 = l2.next

            newNode=newNode.next
        newNode.next = l1 or l2
        return out.next



def main():
    L1=stringToListNode([1,2,4])# 将输入的元组转换为链表
    L2=stringToListNode([1,3,5])# 将输入的元组转换为链表
    solution=Solution().mergeTwoLists(L1,L2)
    print(listNodeToString(solution))


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Runtime: 36 ms, faster than 98.96% of Python3 online submissions forMerge Two Sorted Lists.
- Memory Usage: 13.2 MB, less than 41.08% of Python3 online submissions for Merge Two Sorted Lists.