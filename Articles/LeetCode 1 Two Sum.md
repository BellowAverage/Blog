# LeetCode 1: Two Sum

Created: September 19, 2024 2:05 PM
Labels & Language: C++, LeetCode, Python
Source: https://leetcode.com/problems/two-sum/description/

## Description

https://leetcode.com/problems/two-sum/description/

Given an array of integers `nums` and an integer `target`, return *indices of the two numbers such that they add up to `target`*.

You may assume that each input would have ***exactly* one solution**, and you may not use the *same* element twice.

You can return the answer in any order.

## LeetCode 1: Two Sum

This approach uses a **hashmap** (`dictionary` in Python, `unordered_map` in C++) to efficiently find two numbers that sum up to the target.

The algorithm iterates through the list once, calculating the **complement** (target minus the current number) for each element. 

- If the complement is already in the hashmap, the solution is found, and the indices are returned.
- Otherwise, the current number and its index are stored in the hashmap for future reference.

This allows the solution to find the pair in a single pass.

### Complexity

- **Time Complexity**: O(n) - single pass through the list.
- **Space Complexity**: O(n) - due to the hashmap storage.

### Solution → Python

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        Hashmap = {}

        for i in range(len(nums)):
            compliment = target - nums[i]
            if compliment in Hashmap:
                return [Hashmap[compliment], i]
            Hashmap[nums[i]] = i

        return []
```

### Solution → C++

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        Hashmap = {}

        for i in range(len(nums)):
            compliment = target - nums[i]
            if compliment in Hashmap:
                return [Hashmap[compliment], i]
            Hashmap[nums[i]] = i

        return []
```