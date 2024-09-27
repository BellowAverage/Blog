# Algorithm: Quick Sort

Created: September 22, 2024 6:02 PM
Labels & Language: C++, LeetCode, Python

## Description

https://www.youtube.com/watch?v=Vtckgz38QHs
This note elaborates on major sorting algorithms. The focus here is on **Quick Sort**.

## Quick Sort

Quick Sort is a highly efficient, comparison-based sorting algorithm. It works by selecting a "pivot" element from the array and partitioning the other elements into two subarrays: those less than the pivot and those greater than or equal to the pivot. The pivot is then placed in its correct position, and the process is recursively applied to the subarrays.

### Complexity

- **Time Complexity: O(**n log n**)**
1. Worst-case: O(n²) – occurs when the pivot is consistently chosen as the smallest or largest element, leading to unbalanced partitions.
2. Best-case: O(n log n) – occurs when the pivot divides the array into nearly equal parts.
3. Average-case: O(n log n)
- **Space Complexity: O(**log n**)**
1. O(log n) – due to the recursion stack in the best case.
2. O(n) – in the worst case, when the recursion depth becomes large (due to unbalanced partitions).

### Solution → C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

// Utility function to swap two elements
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}

// Partition function to find the pivot position
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];  // Choosing the rightmost element as pivot
    int i = low - 1;

    // Re-arrange elements based on pivot
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

// Quick Sort function
void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        // Find the pivot element
        int pi = partition(arr, low, high);

        // Recursively sort the left and right subarrays
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main() {
    vector<int> arr = {10, 7, 8, 9, 1, 5};

    quickSort(arr, 0, arr.size() - 1);

    cout << "Sorted array: \n";
    for (int i = 0; i < arr.size(); i++)
        cout << arr[i] << " ";
    cout << endl;

    return 0;
}

```

### Solution → Python

```python
# Partition function to find the pivot position
def partition(arr, low, high):
    pivot = arr[high]  # Choosing the rightmost element as pivot
    i = low - 1

    # Re-arrange elements based on pivot
    for j in range(low, high):
        if arr[j] < pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]

    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

# Quick Sort function
def quick_sort(arr, low, high):
    if low < high:
        # Find the pivot element
        pi = partition(arr, low, high)

        # Recursively sort the left and right subarrays
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)

# Example usage
arr = [10, 7, 8, 9, 1, 5]
quick_sort(arr, 0, len(arr) - 1)
print("Sorted array:", arr)

```