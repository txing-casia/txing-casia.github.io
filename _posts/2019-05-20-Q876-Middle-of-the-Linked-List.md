---
layout:     post
title:      "Q876 Middle of the Linked List"
subtitle:   ""
date:       2019-05-20
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Linked List
    - Easy
---

# [Middle of the Linked List](<https://leetcode.com/problems/middle-of-the-linked-list/>)

## Question

> Given a non-empty, singly linked list with head node `head`, return a middle node of linked list.
>
> If there are two middle nodes, return the second middle node.

> **Example 1:**
> Input: [1,2,3,4,5]
> Output: Node 3 from this list (Serialization: [3,4,5])
> The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
> Note that we returned a ListNode object ans, such that:
> ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.

> **Example 2:**
> Input: [1,2,3,4,5,6]
> Output: Node 4 from this list (Serialization: [4,5,6])
> Since the list has two middle nodes with values 3 and 4, we return the second one.

### Approach 1: Output to Array

**Intuition and Algorithm**

Put every node into an array `A` in order. Then the middle node is just `A[A.length // 2]`, since we can retrieve each node by index.

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def middleNode(self, head):
        A = [head]
        while A[-1].next:
            A.append(A[-1].next)
        return A[len(A) // 2]
        #return head.val[len(head.val) // 2]


def main():
    head=ListNode([1,2,3,4,5])
    print(Solution().middleNode(head))


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: *O*(*N*), where *N* is the number of nodes in the given list.
- Space Complexity: *O*(*N*), the space used by `A`. 

---

### Approach 2: Fast and Slow Pointer

**Intuition and Algorithm**

When traversing the list with a pointer `slow`, make another pointer `fast` that traverses twice as fast. When `fast` reaches the end of the list, `slow` must be in the middle.

```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution(object):
    def middleNode(self, head):
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

- Time Complexity: *O*(*N*), where *N* is the number of nodes in the given list.
- Space Complexity: *O*(1), the space used by `slow` and `fast`. 

- Runtime: 32 ms, faster than 96.73% of Python3 online submissions forMiddle of the Linked List.
- Memory Usage: 13.2 MB, less than 23.44% of Python3 online submissions for Middle of the Linked List.