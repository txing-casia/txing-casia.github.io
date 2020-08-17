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

### Approach 1: Iterative

**Intuition and Algorithm**

Assume that we have linked list `1 → 2 → 3 → Ø`, we would like to change it to `Ø ← 1 ← 2 ← 3`.

While you are traversing the list, change the current node's next pointer to point to its previous element. Since a node does not have reference to its previous node, you must store its previous element beforehand. You also need another pointer to store the next node before changing the reference. Do not forget to return the new head reference at the end!

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

def main():
    head=stringToListNode([1,2,3,4,5])# 将输入的元组转换为链表
    print(listNodeToString(Solution().reverseList(head)))


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time complexity : *O*(*n*). Assume that *n* is the list's length, the time complexity is *O*(*n*).

- Space complexity : *O*(1).

  

- Runtime: 28 ms, faster than 99.98% of Python3 online submissions forReverse Linked List.

- Memory Usage: 14.5 MB, less than 50.22% of Python3 online submissions for Reverse Linked List.

---

### Approach 2: Recursive

**Intuition and Algorithm**

The recursive version is slightly trickier and the key is to work backwards. Assume that the rest of the list had already been reversed, now how do I reverse the front part? Let's assume the list is: $$n_1 \to … \to n_{k-1} \to n_k \to n_{k+1} \to … \to n_m \to \phi​$$

Assume from node $$n_{k+1}$$ to $$n_m$$ had been reversed and you are at node $$n_k$$.

$$n_1 \to … \to n_{k-1} \to n_k \to n_{k+1} \leftarrow … \leftarrow n_m$$

We want $$n_{k+1}$$’s next node to point to $$n_k$$.

So, $$n_k$$.next.next = $$n_k​$$;

Be very careful that $$n_1$$'s next must point to $$\phi​$$. If you forget about this, your linked list has a cycle in it. This bug could be caught if you test your code with a linked list of size 2.

```python
class Solution:
    def reverseList(self, head):
        if head==None or head.next==None:
            return head
        p=head.next # 设定p是下一个node
        p=Solution().reverseList(p)
        head.next.next=head
        head.next=None
        return p
```

**Complexity analysis**

- Time complexity :  *O*(*n*). Assume that *n* is the list's length, the time complexity is *O*(*n*).

- Space complexity : *O*(*n*). The extra space comes from implicit stack space due to recursion. The recursion could go up to *n* levels deep.

  

- Runtime: 44 ms, faster than 69.21% of Python3 online submissions forReverse Linked List.
- Memory Usage: 19.1 MB, less than 11.36% of Python3 online submissions for Reverse Linked List.