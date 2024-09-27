# LeetCode 217: Contains Duplicate

Created: September 19, 2024 5:26 PM
Labels & Language: C++, LeetCode, Python

## Description

https://leetcode.com/problems/contains-duplicate/description/

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

## LeetCode 217: Contains Duplicate

### Approach: Hash Set

The problem is to determine if any integer appears at least twice in the given integer array `nums`. To efficiently check for duplicates, we can use a hash set. The main idea is to leverage the properties of a hash set, which allows for fast membership testing and ensures that all elements are unique.

1. **Initialize a Hash Set**
    - Create an empty hash set (or unordered set in C++) to store the unique elements encountered while iterating through the array.
2. **Iterate Through the Array**
    - Loop through each element in the array `nums`.
    - For each element, check if it is already present in the hash set.
3. **Check for Duplicates**
    - If the current element is found in the hash set (using `find` or a similar method), it means that this value has been seen before, and we can immediately return `true`.
    - If the current element is not in the hash set, insert it into the set for future checks.
4. **Return Result**
    - If the loop completes without finding any duplicates, return `false`, indicating that all elements in the array are distinct.

### Complexity

- **Time Complexity: O(n)**

The algorithm iterates through the array once, and each insertion and lookup operation in the hash set takes average O(1) time. Therefore, the overall complexity is linear with respect to the number of elements in the array.

- **Space Complexity: O(n)**

In the worst case, if all elements are distinct, we would store all n elements in the hash set, leading to linear space usage.

### Solution → C++

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> hashTable;

        for (int i = 0; i < nums.size(); i ++) {
            if (hashTable.find(nums[i]) != hashTable.end())
            {
                return true;
            }
            else
            {
                hashTable.insert(nums[i]);
            }
        }

        return false;

    }
};
```

### Solution → Python

```python
class Solution:
    def containsDuplicate(self, nums):
        hash_table = set()

        for num in nums:
            if num in hash_table:
                return True
            else:
                hash_table.add(num)

        return False
```