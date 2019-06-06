---
layout:     post
title:      "Q804 Unique Morse Code Words"
subtitle:   "nested loop in only one row"
date:       2019-06-04
author:     "Txing"
header-img: "img/post-bg-py.jpg"
tags:
    - Algorithm
    - Python
    - Easy
    - String
---

# [Unique Morse Code Words](<https://leetcode.com/problems/unique-morse-code-words/>)

## Question 

> International Morse Code defines a standard encoding where each letter is mapped to a series of dots and dashes, as follows: `"a"` maps to `".-"`, `"b"` maps to `"-..."`, `"c"` maps to `"-.-."`, and so on.
>
> For convenience, the full table for the 26 letters of the English alphabet is given below:
>
> `[".-","-...","-.-.","-..",".","..-.","--.","....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-","...-",".--","-..-","-.--","--.."]`
>
> Now, given a list of words, each word can be written as a concatenation of the Morse code of each letter. For example, "cba" can be written as "-.-..--...", (which is the concatenation "-.-." + "-..." + ".-"). We'll call such a concatenation, the transformation of a word.
>
> Return the number of different transformations among all words we have.

> **Example 1:**
> Input: words = ["gin", "zen", "gig", "msg"]
> Output: 2
> Explanation: 
> The transformation of each word is:
> "gin" -> "--...-."
> "zen" -> "--...-."
> "gig" -> "--...--."
> "msg" -> "--...--."
>
> There are 2 different transformations, "--...-." and "--...--.".

## Approach 1: Hash Set

**Intuition and Algorithm**

We can transform each `word` into it's Morse Code representation.

After, we put all transformations into a set `seen`, and return the size of the set.

```python
class Solution(object):
    def uniqueMorseRepresentations(self, words):
        MORSE = [".-","-...","-.-.","-..",".","..-.","--.",
                 "....","..",".---","-.-",".-..","--","-.",
                 "---",".--.","--.-",".-.","...","-","..-",
                 "...-",".--","-..-","-.--","--.."]

        seen = {"".join(MORSE[ord(c) - ord('a')] for c in word)
                for word in words}

        return len(seen)



def main():
    Ain=["gin", "zen", "gig", "msg"]
    solution=Solution().uniqueMorseRepresentations(Ain)
    print(solution)


if __name__ == "__main__":
    main()
```

**Complexity Analysis**

- Time Complexity: *O*(*S*), where *S* is the sum of the lengths of words in `words`. We iterate through each character of each word in `words`.
- Space Complexity: *O*(*S*).
- Runtime: 20 ms, faster than 95.59% of Python online submissions for Unique Morse Code Words.
- Memory Usage: 11.8 MB, less than 46.42% of Python online submissions for Unique Morse Code Words.

