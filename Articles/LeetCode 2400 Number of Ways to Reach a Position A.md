# LeetCode 2400: Number of Ways to Reach a Position After Exactly k Steps

Created: September 12, 2024 1:48 PM
Labels & Language: LeetCode, Medium
Source: https://leetcode.com/problems/number-of-ways-to-reach-a-position-after-exactly-k-steps/

## Description

LeetCode 2400 can be solved by [Binary Search Tree](Binary%20Search%20Tree%20a91d9413bee0423fb608f131108b709a.md). But it’s too slow when it comes to a large input. The typical solution is to mathematically calculate the result using the theory of combination.

## LeetCode 2400

You are given two **positive** integers `startPos` and `endPos`. Initially, you are standing at position `startPos` on an **infinite** number line. With one step, you can move either one position to the left, or one position to the right.

Given a positive integer `k`, return *the number of **different** ways to reach the position* `endPos` *starting from* `startPos`*, such that you perform **exactly*** `k` *steps*. Since the answer may be very large, return it **modulo** `109 + 7`.

Two ways are considered different if the order of the steps made is not exactly the same.

**Note** that the number line includes negative integers.

**Example 1:**

```
Input: startPos = 1, endPos = 2, k = 3
Output: 3
Explanation: We can reach position 2 from 1 in exactly 3 steps in three ways:
- 1 -> 2 -> 3 -> 2.
- 1 -> 2 -> 1 -> 2.
- 1 -> 0 -> 1 -> 2.
It can be proven that no other way is possible, so we return 3.
```

**Example 2:**

```
Input: startPos = 2, endPos = 5, k = 10
Output: 0
Explanation: It is impossible to reach position 5 from position 2 in exactly 10 steps.

```

**Constraints:**

- `1 <= startPos, endPos, k <= 1000`

## Solution 1 —> Binary Search Tree

```python
class Solution:
    def numberOfWays(self, startPos: int, endPos: int, k: int) -> int:
        class Node:
            
            res = 0

            def __init__(self, value):
                self.value = value
                self.left = None
                self.right = None
            
            def create(self, k, endPos):
                self.left = Node(self.value-1)
                self.right = Node(self.value+1)
                k -= 1
                if k!=0:
                    self.left.create(k, endPos)
                    self.right.create(k, endPos)
                else:
                    if self.left.value == endPos:
                        Node.res += 1
                    if self.right.value == endPos:
                        Node.res += 1
            
        tree = Node(startPos)
        tree.create(k, endPos)
        return Node.res
```

## Other sources & Reference

- See how Binary Search Tree works here —> [Binary Search Tree](Binary%20Search%20Tree%20a91d9413bee0423fb608f131108b709a.md)
- LeetCode 2400 U.S Website URL —>
    
    [Number of Ways to Reach a Position After Exactly k Steps - LeetCode](https://leetcode.com/problems/number-of-ways-to-reach-a-position-after-exactly-k-steps/)