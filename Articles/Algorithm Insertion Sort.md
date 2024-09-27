# Algorithm: Insertion Sort

Created: September 22, 2024 3:48 PM
Labels & Language: C++, LeetCode, Python

## Description

https://www.youtube.com/watch?v=8mJ-OhcfpYg
This note elaborates on major sorting algorithms in C++. This page focuses on **Insertion Sorting**.

## Insertion Sort

Insertion Sort is a simple, comparison-based sorting algorithm. It builds the sorted array one item at a time by repeatedly inserting the next element from the unsorted portion into its correct position in the sorted portion. This method is similar to how you would sort playing cards in your hand.

### Complexity

- **Time Complexity: O(n²)**
1. Worst-case: O(n²) – occurs when the array is sorted in reverse order, requiring the algorithm to move each element to the front.
2. Best-case: O(n) – occurs when the array is already sorted, as only one comparison is made per element.
3. Average-case: O(n²)
- **Space Complexity: O(1)**

Insertion Sort is an in-place sorting algorithm, meaning it only requires a constant amount of extra space.

### Solution → C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

void insertionSort(vector<int>& arr) {
    int n = arr.size();

    // Traverse from the second element to the last
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;

        // Move elements of arr[0..i-1] that are greater than key to one position ahead
        // of their current position
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;  // Place the key at the correct position
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6};
    
    insertionSort(arr);

    cout << "Sorted array: \n";
    for (int i = 0; i < arr.size(); i++)
        cout << arr[i] << " ";
    cout << endl;

    return 0;
}
```

### Solution → Python

```python
def insertion_sort(arr):
    n = len(arr)

    # Traverse from the second element to the last
    for i in range(1, n):
        key = arr[i]
        j = i - 1

        # Move elements of arr[0..i-1] that are greater than key to one position ahead
        # of their current position
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key  # Place the key at the correct position

# Example usage
arr = [12, 11, 13, 5, 6]
insertion_sort(arr)
print("Sorted array:", arr)
```