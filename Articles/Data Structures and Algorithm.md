# Data Structures and Algorithm

Created: September 12, 2024 1:48 PM
Source: https://www.hello-algo.com/

# Introduction

Data Structures and Algorithm is a subject that deserves a second look, even if I have studied it years ago and felt rather boring about it at the moment.

[Hello 算法](https://www.hello-algo.com/)

# Chapter 1 Encounter with Algorithms

- Looking up a dictionary: **Binary Search**
- Playing cards sorting process: **Insertion Sort**

Very efficient when dealing with small datasets

- Change making process: **Greedy**

# Chapter 2 Complexity Analysis

- Complexity analysis reflects the relationship between the time and space resources required for algorithm execution and the size of the input data. **It describes the trend of growth in the time and space required by the algorithm as the size of the input data increases.**

## 2.2 Iteration and Recursion

### Iteration

- Iteration is a control structure for **repeatedly performing a task**.
- In iteration, a program repeats a block of code as long as a certain condition is met **until this condition is no longer satisfied**.

### Recursion

- Recursion is an algorithmic strategy where a function solves a problem by **calling itself**. It primarily involves two phases:
- Calling: This is where the program repeatedly calls itself, often with progressively **smaller or simpler arguments**, moving towards the "termination condition."
- Returning: Upon triggering the "**termination condition**," the program begins to return from the deepest recursive function, aggregating the results of each layer.
From an implementation perspective, recursive code mainly includes three elements.
1. Termination Condition: Determines when to switch from "calling" to "returning." **This happens in the deepest layer, though the logic that is the codes is executed every layer.**
2. Recursive Call: Corresponds to "calling," where the function calls itself, usually with smaller or more simplified parameters.
3. Return Result: Corresponds to "returning," where the result of the current recursion level is returned to the previous layer
- The function's context data is stored in a memory area called "stack frame space" and is only released after the function returns. Therefore, **recursion generally consumes more memory space than iteration**.
- Recursive calls introduce additional overhead. **Hence, recursion is usually less time-efficient than loops.**
- Interestingly, if a function **performs its recursive call as the very last step before returning**, it can be optimized by the compiler or interpreter to be as space efficient as iteration. This scenario is known as **tail recursion**.
- When dealing with algorithms related to "**divide and conquer**", **recursion often offers a more intuitive approach and more readable code than iteration**.

## 2.3 Time Complexity

- Time complexity analysis does not count the algorithm's run time, **but rather the growth trend of the run time as the data volume increases**.
- In essence, time complexity analysis is about finding the **asymptotic upper bound** of the **number of operations**.

$$
O(1) < O(\log n) < O(n) < O(n \log n) < O(n^2) < O(2^n) < O(n!)
$$

- Calculating time complexity involves two steps: first **counting the number of operations**, then **determining the asymptotic upper bound**.

# Chapter 3 Data Structures

## 3.1 Classification of Data Structures

### Logical structure: linear and non-linear

- **Linear data structures**: Arrays, Linked Lists, Stacks, Queues, Hash Tables.
- **Non-linear data structures**: Trees, Heaps, Graphs, Hash Tables.

![ Linear and non-linear data structures](Data%20Structures%20and%20Algorithm%20d4d8fb7d710d49d1a8a05e175f8bbaa2/classification_logic_structure.png)

 Linear and non-linear data structures

### **Physical structure: contiguous and dispersed**

- The physical structure can be divided into **contiguous space storage (arrays)** and **non-contiguous space storage (linked lists).**

![ Contiguous space storage and dispersed space storage](Data%20Structures%20and%20Algorithm%20d4d8fb7d710d49d1a8a05e175f8bbaa2/classification_phisical_structure.png)

 Contiguous space storage and dispersed space storage

- **All data structures are implemented based on arrays, linked lists, or a combination of both**.
- Data structures implemented based on **arrays** are also called “**Static Data Structures**,” meaning their length cannot be changed after initialization.
- Conversely, those based on **linked lists** are called “**Dynamic Data Structures**,” which can still adjust their size during program execution.

# Chapter 4 Array and Linked List