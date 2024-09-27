# LeetCode 238: Product of Array Except Self

Created: September 21, 2024 8:16 PM
Labels & Language: C++, LeetCode, Python

## Description

Given an integer array `nums`, return *an array* `answer` *such that* `answer[i]` *is equal to the product of all the elements of* `nums` *except* `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a **32-bit** integer.

You must write an algorithm that runs in `O(n)` time and without using the division operation.

## LeetCode 238: Product of Array Except Self

### Approach

We can solve this problem using two passes through the array. In the first pass, we compute the prefix products, and in the second pass, we compute the suffix products while updating the result.

1. **Prefix Products:** Create an output array where each element at index `i` contains the product of all elements to the left of `i`.
2. **Suffix Products:** Traverse the array from the end, and for each element, multiply it by the product of all elements to the right of `i`.

### Complexity

- **Time Complexity: O(n)**

We traverse the array a couple of times.

- **Space Complexity: O(1)**

We use the output array for results, but no additional space is needed for calculations.

### Solution → C++

```cpp
#include <vector>

std::vector<int> productExceptSelf(std::vector<int>& nums) {
    int n = nums.size();
    std::vector<int> output(n, 1);
    
    // Calculate prefix products
    for (int i = 1; i < n; i++) {
        output[i] = output[i - 1] * nums[i - 1];
    }
    
    // Calculate suffix products and final result
    int suffixProduct = 1;
    for (int i = n - 1; i >= 0; i--) {
        output[i] *= suffixProduct;
        suffixProduct *= nums[i];
    }
    
    return output;
}
```

### Solution → Python

```python
def product_except_self(nums):
    n = len(nums)
    output = [1] * n
    
    # Calculate prefix products
    for i in range(1, n):
        output[i] = output[i - 1] * nums[i - 1]
    
    # Calculate suffix products and final result
    suffix_product = 1
    for i in range(n - 1, -1, -1):
        output[i] *= suffix_product
        suffix_product *= nums[i]
    
    return output
```