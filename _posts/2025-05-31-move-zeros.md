---
title: "Move Zeros"
date: 2025-05-31 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/move-zeroes/?envType=problem-list-v2&envId=array>

### Description
Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.

Note that you must do this in-place without making a copy of the array.

### Example
Input: nums = [0,1,0,3,12]
Output: [1,3,12,0,0]

### Solution Code
To solve this problem, we need to move all the zeros in an array to the end while maintaining the relative order of the non-zero elements. The solution should be done in-place, meaning we cannot use additional space for another array.

### Approach
1. **Two Pointers Technique**: We can use two pointers to achieve this. The first pointer (let's call it `nonZeroIndex`) keeps track of the position where the next non-zero element should be placed. The second pointer (let's call it `current`) iterates through the array to find non-zero elements.
2. **Process**:
   - Initialize `nonZeroIndex` to 0.
   - Iterate through the array with `current`. For each non-zero element encountered, place it at the `nonZeroIndex` position and increment `nonZeroIndex`.
   - After processing all elements, fill the remaining positions from `nonZeroIndex` to the end of the array with zeros.

This approach ensures that all non-zero elements retain their original order, and zeros are moved to the end efficiently.

### Solution Code
```c
#include <stdio.h>

void moveZeroes(int* nums, int numsSize) {
    int nonZeroIndex = 0;

    // Move all non-zero elements to the front
    for (int current = 0; current < numsSize; current++) {
        if (nums[current] != 0) {
            nums[nonZeroIndex++] = nums[current];
        }
    }

    // Fill the remaining positions with zeros
    for (int i = nonZeroIndex; i < numsSize; i++) {
        nums[i] = 0;
    }
}

int main() {
    int nums1[] = {0, 1, 0, 3, 12};
    int size1 = sizeof(nums1) / sizeof(nums1[0]);
    moveZeroes(nums1, size1);
    printf("Result: ");
    for (int i = 0; i < size1; i++) {
        printf("%d ", nums1[i]);
    }
    printf("\n");

    int nums2[] = {0};
    int size2 = sizeof(nums2) / sizeof(nums2[0]);
    moveZeroes(nums2, size2);
    printf("Result: ");
    for (int i = 0; i < size2; i++) {
        printf("%d ", nums2[i]);
    }
    printf("\n");

    return 0;
}
```

### Explanation
1. **Initialization**: We start with `nonZeroIndex` set to 0, which will keep track of where the next non-zero element should be placed.
2. **First Loop (Moving Non-Zero Elements)**: As we iterate through the array with `current`, whenever we encounter a non-zero element, we place it at `nonZeroIndex` and then increment `nonZeroIndex`. This ensures all non-zero elements are moved to the front in their original order.
3. **Second Loop (Filling Zeros)**: After all non-zero elements are placed, we fill the remaining positions from `nonZeroIndex` to the end of the array with zeros. This step ensures all zeros are moved to the end while maintaining the relative order of non-zero elements.

This approach efficiently moves all zeros to the end of the array in O(n) time with O(1) additional space, meeting the problem constraints.
