---
title: "Converted Sorted Array to Binary Search Tree"
date: 2025-05-24 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/description/?envType=problem-list-v2&envId=array>

### Description
Given an integer array nums where the elements are sorted in ascending order, convert it to a height-balanced binary search tree.

### Example
Input: nums = [-10,-3,0,5,9]
Output: [0,-3,9,-10,null,5]
Explanation: [0,-10,5,null,-3,null,9] is also accepted:

Here's a C solution to convert a sorted array into a height-balanced binary search tree (BST):

### Solution Code

```c
#include <stdio.h>
#include <stdlib.h>

// Definition for a binary tree node
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
};

struct TreeNode* sortedArrayToBST(int* nums, int numsSize) {
    if (numsSize <= 0) return NULL;

    // Find middle element
    int mid = numsSize / 2;

    // Create root node with middle element
    struct TreeNode* root = (struct TreeNode*)malloc(sizeof(struct TreeNode));
    root->val = nums[mid];

    // Recursively build left and right subtrees
    root->left = sortedArrayToBST(nums, mid);
    root->right = sortedArrayToBST(nums + mid + 1, numsSize - mid - 1);

    return root;
}

// Helper function to print tree in-order
void printTree(struct TreeNode* root) {
    if (root == NULL) return;
    printTree(root->left);
    printf("%d ", root->val);
    printTree(root->right);
}

int main() {
    int nums[] = {-10, -3, 0, 5, 9};
    int size = sizeof(nums)/sizeof(nums[0]);

    struct TreeNode* root = sortedArrayToBST(nums, size);

    printf("In-order traversal of BST: ");
    printTree(root);

    return 0;
}
```

### Explanation

1. **Base Case**: If the array is empty, return NULL.

2. **Middle Element Selection**:
   - Find the middle index `mid = numsSize / 2`
   - This ensures the tree remains height-balanced

3. **Root Creation**:
   - Create a new tree node with the middle element as its value

4. **Recursive Construction**:
   - Left subtree: All elements before the middle element
   - Right subtree: All elements after the middle element
   - This recursive division guarantees the BST property

5. **Example**:
   - For input `[-10, -3, 0, 5, 9]`:
     ```
         0
        / \
      -3   9
      /   /
    -10   5
     ```

### Key Features

- **Time Complexity**: O(n) - Each element is processed exactly once
- **Space Complexity**: O(log n) - Recursion stack space for balanced tree
- **Height Balance**: Tree height is minimized by always choosing the middle element
- **BST Property Maintained**: Left subtree contains smaller elements, right contains larger elements

This solution efficiently constructs a height-balanced BST by leveraging the sorted nature of the input array through recursive division.
