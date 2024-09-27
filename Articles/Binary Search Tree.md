# Binary Search Tree

Created: September 12, 2024 1:48 PM
Labels & Language: Data Structures, Python
Source: https://www.youtube.com/watch?v=DlWxqU3LLpY

## Description

Taught in school years ago about data structure, using C++. Later I found myself using Python more often. So I searched again on Python’s version of data structure and tried to relearn.

This displays graphically how Binary Search Tree works.

[Binary Search Tree](https://www.cs.usfca.edu/~galles/visualization/BST.html)

## Binary Search Tree

### 1. Create the tree node class.

```python
class TreeNode:

    def __init__(self, value):
        self.left = None
        self.right = None
        self.value = value
```

### 2. Insert elements into the Binary Tree.

Assume that the key determination rule of a binary tree is, if the inserting value is larger than its father, put it on the left side; else put it on the right side. To do the insertion —>

1. There is no node on the active node —> Create the tree node class on that node using the inserting value.
2. This is a node on it —> Create a tree node class on that node’s position. Then do it again, using the inserting value —> **recursion.**

```python
def insert(self, value):
        if value < self.value:
            if self.left is None:
                self.left = TreeNode(value)
            else:
                self.left.insert(value)
        else:
            if self.right is None:
                self.right = TreeNode(value)
            else:
                self.right.insert(value)
```

### 3. Traversal —> in-order, pre-order, and post-order

There are 3 types of traversals in an ordinary Binary Search Tree. Notice that the Chinese ways of naming these methods are somehow … strange. It’s easy to confuse at the beginning.

| In-order Traversal | 中序遍历 | Return value in the middle of the each side’s recursion. |
| --- | --- | --- |
| Pre-order Traversal | 先序遍历 | Return value upfront before the recursions. |
| Post-order Traversal | 后序遍历 | Return value after both sides’ recursions. |

```python
def inorder_traversal(self):
        # as left as possible
        if self.left:
            # remember this line does not terminate this function!!
            self.left.inorder_traversal()
        print(self.value)
        if self.right:
            self.right.inorder_traversal()

    def preorder_traversal(self):
        print(self.value)
        if self.left:
            self.left.preorder_traversal()
        if self.right:
            self.right.preorder_traversal()

    def postorder_traversal(self):
        if self.left:
            self.left.postorder_traversal()
        if self.right:
            self.right.postorder_traversal()
        print(self.value)
```

### 4. Find —> same recursion algorithm

Using Binary Tree as a data structure is of certain assistance when it comes to searching an element. In fact, the worst case searching in an ordinary Binary Tree in terms of time complexity is **linear.** This is because in an ordinary traversal, Binary Tree normally gets rid of half of the invalid directions (averagely one side).

```python
def find(self, value):
        if value < self.value:
            if self.left is None:
                return False
            else:
                self.left.find(value)
        elif value > self.value:
            if self.right is None:
                return False
            else:
                self.right.find(value)
        else:
            return True
```

## Other Sources

- Very helpful tutorial by NeuralNine on YouTube.

[https://www.youtube.com/watch?v=DlWxqU3LLpY](https://www.youtube.com/watch?v=DlWxqU3LLpY)

- [LeetCode 2400](LeetCode%202400%20Number%20of%20Ways%20to%20Reach%20a%20Position%20A%200d8ca20c406e433a87f33d3984aa5cb1.md) is related to this data structure. Although it can be handled through other algorithm, which could be much easier somehow, I still put it here 🥲.