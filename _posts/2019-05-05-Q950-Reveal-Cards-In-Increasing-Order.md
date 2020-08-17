---
layout:     post
title:      "Q950 Reveal Cards In Increasing Order"
subtitle:   "非常简洁美观的代码（用到了popleft和append）"
date:       2019-05-05
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Array
---

# [Reveal Cards In Increasing Order](https://leetcode.com/problems/reveal-cards-in-increasing-order/)

## Question

> In a deck of cards, every card has a unique integer.  You can order the deck in any order you want.
>
> Initially, all the cards start face down (unrevealed) in one deck.
>
> Now, you do the following steps repeatedly, until all cards are revealed:
>
> 1. Take the top card of the deck, reveal it, and take it out of the deck.
> 2. If there are still cards in the deck, put the next top card of the deck at the bottom of the deck.
> 3. If there are still unrevealed cards, go back to step 1.  Otherwise, stop.
>
> Return an ordering of the deck that would reveal the cards in **increasing order.**
>
> The first entry in the answer is considered to be the top of the deck.

> **Example 1**: 
>
> Input: [17,13,11,2,3,5,7]
> Output: [2,13,3,11,5,17,7]
> Explanation: 
> We get the deck in the order [17,13,11,2,3,5,7] (this order doesn't matter), and reorder it.
> After reordering, the deck starts as [2,13,3,11,5,17,7], where 2 is the top of the deck.
> We reveal 2, and move 13 to the bottom.  The deck is now [3,11,5,17,7,13].
> We reveal 3, and move 11 to the bottom.  The deck is now [5,17,7,13,11].
> We reveal 5, and move 17 to the bottom.  The deck is now [7,13,11,17].
> We reveal 7, and move 13 to the bottom.  The deck is now [11,17,13].
> We reveal 11, and move 17 to the bottom.  The deck is now [13,17].
> We reveal 13, and move 17 to the bottom.  The deck is now [17].
> We reveal 17.
> Since all the cards revealed are in increasing order, the answer is correct.

---

### Approach 1: Simulation

**Intuition and Algorithm**

Simulate the revealing process with a deck set to `[0, 1, 2, ...]`. If for example this deck is revealed in the order `[0, 2, 4, ...]` then we know we need to put the smallest card in index `0`, the second smallest card in index `2`, the third smallest card in index `4`, etc.

```python
import collections

class Solution(object):
    def deckRevealedIncreasing(self, deck):
        N = len(deck)
        index = collections.deque(range(N))
        ans = [None] * N

        for card in sorted(deck):
            ans[index.popleft()] = card
            if index:
                index.append(index.popleft())

        return ans


def main():
    A = [17,13,11,2,3,5,7]
    answer=Solution()
    results=answer.deckRevealedIncreasing(A)
    print(results)


if __name__ == "__main__":
    main()
    
    
    
'''
deque()是一种双向队列，同时具有队和栈的性质，可以旋转、移动等。具体用法如下

d = collections.deque([])
d.append('a') # 在最右边添加一个元素，此时 d=deque('a')
d.appendleft('b') # 在最左边添加一个元素，此时 d=deque(['b', 'a'])
d.extend(['c','d']) # 在最右边添加所有元素，此时 d=deque(['b', 'a', 'c', 'd'])
d.extendleft(['e','f']) # 在最左边添加所有元素，此时 d=deque(['f', 'e', 'b', 'a', 'c', 'd'])
d.pop() # 将最右边的元素取出，返回 'd'，此时 d=deque(['f', 'e', 'b', 'a', 'c'])
d.popleft() # 将最左边的元素取出，返回 'f'，此时 d=deque(['e', 'b', 'a', 'c'])
d.rotate(-2) # 向左旋转两个位置（正数则向右旋转），此时 d=deque(['a', 'c', 'e', 'b'])
d.count('a') # 队列中'a'的个数，返回 1
d.remove('c') # 从队列中将'c'删除，此时 d=deque(['a', 'e', 'b'])
d.reverse() # 将队列倒序，此时 d=deque(['b', 'e', 'a'])
'''    
```

