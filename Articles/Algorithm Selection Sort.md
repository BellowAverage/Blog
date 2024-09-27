# Algorithm: Selection Sort

Created: September 22, 2024 2:03 PM
Labels & Language: C++, LeetCode, Python

## Description

https://www.youtube.com/watch?v=EwjnF7rFLns
This note elaborates on major sorting algorithms in C++. This page focuses on **Selection Sorting**.

## Algorithm: Selection Sort

Selection Sort is a simple comparison-based sorting algorithm. It divides the input list into two parts: the sorted part at the left end and the unsorted part at the right end. The algorithm repeatedly selects the smallest (or largest, depending on the order) element from the unsorted portion and swaps it with the first element of the unsorted part, expanding the sorted portion by one element.

### Complexity

- **Time Complexity: O(n²)**
1. Worst-case: O(n²) – occurs when the algorithm has to traverse the entire list for each element.
2. Best-case: O(n²) – even if the list is already sorted, Selection Sort will still perform the same number of comparisons.
3. Average-case: O(n²)
- **Space Complexity: O(1)**

Selection Sort is an in-place sorting algorithm, requiring only a constant amount of extra space.

### Solution → C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

void selectionSort(vector<int>& arr) {
    int n = arr.size();

    // Traverse through the array
    for (int i = 0; i < n - 1; i++) {
        // Find the minimum element in the unsorted part
        int min_idx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        // Swap the found minimum element with the first element of the unsorted part
        swap(arr[i], arr[min_idx]);
    }
}

int main() {
    vector<int> arr = {64, 25, 12, 22, 11};
    
    selectionSort(arr);

    cout << "Sorted array: \n";
    for (int i = 0; i < arr.size(); i++)
        cout << arr[i] << " ";
    cout << endl;

    return 0;
}

```

### Solution → Python

```python
def selection_sort(arr):
    n = len(arr)

    # Traverse through the array
    for i in range(n - 1):
        # Find the minimum element in the unsorted part
        min_idx = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        
        # Swap the found minimum element with the first element of the unsorted part
        arr[i], arr[min_idx] = arr[min_idx], arr[i]

# Example usage
arr = [64, 25, 12, 22, 11]
selection_sort(arr)
print("Sorted array:", arr)

```