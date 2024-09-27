# Algorithm: Bubble Sorting

Created: September 22, 2024 12:59 PM
Labels & Language: C++, LeetCode, Python

## Description

https://www.youtube.com/watch?v=Dv4qLJcxus8

This note elaborates all major sorting algorithm in C++. This page focuses on Bubble Sorting.

## Bubble Sort

Bubble Sort is a simple comparison-based sorting algorithm. It repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. This process is repeated until the list is sorted.

### Complexity

- **Time Complexity: O(n²)**
1. Worst-case: O(n²) – occurs when the list is in reverse order.
2. Best-case: O(n) – occurs when the list is already sorted (with an optimization that stops the algorithm when no swaps are made).
3. Average-case: O(n²)
- **Space Complexity: O(1)**

Bubble Sort is an in-place sorting algorithm, meaning it only requires a constant amount of extra space.

### Solution → C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    bool swapped;
    
    // Traverse through all elements
    for (int i = 0; i < n - 1; i++) {
        swapped = false;
        
        // Last i elements are already sorted
        for (int j = 0; j < n - i - 1; j++) {
            // Swap if the element is greater than the next
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }

        // If no two elements were swapped by inner loop, then break
        if (!swapped)
            break;
    }
}

int main() {
    vector<int> arr = {64, 34, 25, 12, 22, 11, 90};
    
    bubbleSort(arr);

    cout << "Sorted array: \n";
    for (int i = 0; i < arr.size(); i++)
        cout << arr[i] << " ";
    cout << endl;
    
    return 0;
}

```

### Solution → Python

```python
def bubble_sort(arr):
    n = len(arr)
    
    # Traverse through all elements
    for i in range(n - 1):
        swapped = False
        
        # Last i elements are already sorted
        for j in range(n - i - 1):
            # Swap if the element is greater than the next
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True

        # If no two elements were swapped by inner loop, then break
        if not swapped:
            break

# Example usage
arr = [64, 34, 25, 12, 22, 11, 90]
bubble_sort(arr)
print("Sorted array:", arr)

```