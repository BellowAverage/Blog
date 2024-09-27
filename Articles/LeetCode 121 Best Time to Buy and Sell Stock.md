# LeetCode 121: Best Time to Buy and Sell Stock

Created: September 19, 2024 2:23 PM
Labels & Language: C++, LeetCode, Python

## Description

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

## LeetCode 121: Best Time to Buy and Sell Stock

### Approach: Sliding Window

- **Initialization:**
    - `leftPtr` (left pointer) is initialized to 0. This pointer represents the day on which the stock is bought.
    - `rightPtr` (right pointer) is initialized to 1. This pointer represents the day on which the stock is sold.
    - `maxProfit` is initialized to 0, which will store the maximum profit found.
- **Iterative Process:**
    - The `while` loop continues as long as `rightPtr` is less than the size of the `prices` array.
    - Inside the loop, the algorithm checks if the price on the `rightPtr` day is higher than the price on the `leftPtr` day:
        - **If True:** Calculate the potential profit as `prices[rightPtr] - prices[leftPtr]`. Update `maxProfit` to the maximum of the current `maxProfit` and the potential profit.
        - **If False:** Move the `leftPtr` to `rightPtr`. This means the new day is considered as a potential new buying day since the previous price was higher.
    - Increment `rightPtr` to explore the next day as a potential selling day.
- **Return the Result:**
    - After the loop completes, `maxProfit` will contain the maximum profit achievable. Return this value.

### Complexity

- **Time Complexity: O(n)**

The algorithm processes each element of the `prices` array exactly once.

- **Space Complexity: O(1)**

The solution uses a constant amount of extra space.

### Solution → C++

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {

        int leftPtr = 0;
        int rightPtr = 1;
        int maxProfit = 0;

        while (rightPtr < prices.size()) {

            if (prices[rightPtr] > prices[leftPtr]) {
                maxProfit = max(maxProfit, prices[rightPtr] - prices[leftPtr]);
            }

            else {
                leftPtr = rightPtr;
            }

            rightPtr ++;
            
        }

        return maxProfit;
    }

};
```

### Solution → Python

```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        leftPtr = 0
        rightPtr = 1
        maxProfit = 0

        while rightPtr < len(prices):
            if prices[rightPtr] > prices[leftPtr]:
                potentialProfit = prices[rightPtr] - prices[leftPtr]
                maxProfit = maxProfit if maxProfit > potentialProfit else potentialProfit
            
            else:
                leftPtr = rightPtr
            
            rightPtr += 1
        
        return maxProfit
```