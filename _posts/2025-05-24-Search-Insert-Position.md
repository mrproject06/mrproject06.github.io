---
title: "Search Insert Position"
date: 2025-05-24 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/search-insert-position/description/?envType=problem-list-v2&envId=array>

### Description
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with O(log n) runtime complexity.


### Example
Input: nums = [1,3,5,6], target = 5
Output: 2


Here's a C solution that implements binary search to find the insertion position or the index of the target in O(log n) time:

### Solution Code
```c
int searchInsert(int* nums, int numsSize, int target) {
    int left = 0;
    int right = numsSize - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;

        if (nums[mid] == target) {
            return mid;  // Target found
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    // If target not found, left is the insertion position
    return left;
}
```

### Explanation

1. **Binary Search Setup**:
   - Initialize `left` to 0 (start of array) and `right` to `numsSize - 1` (end of array).

2. **Binary Search Loop**:
   - Calculate `mid` as the middle index between `left` and `right`.
   - If `nums[mid]` equals the target, return `mid` immediately (target found).
   - If `nums[mid]` is less than the target, search the right half by setting `left = mid + 1`.
   - If `nums[mid]` is greater than the target, search the left half by setting `right = mid - 1`.

3. **Insertion Position**:
   - If the loop completes without finding the target, `left` will point to the position where the target should be inserted to maintain order.

### Example Usage

```c
#include <stdio.h>

int main() {
    int nums[] = {1, 3, 5, 6};
    int target = 5;
    int size = sizeof(nums) / sizeof(nums[0]);

    int index = searchInsert(nums, size, target);

    printf("Index: %d\n", index);
    return 0;
}
```

### Output
For the input array `{1, 3, 5, 6}` and `target = 5`, the output will be:
```
Index: 2
```

### Key Features
- **Time Complexity**: O(log n) - Binary search ensures logarithmic time complexity.
- **Space Complexity**: O(1) - Uses constant extra space.
- **Correctness**:
  - Returns the index of the target if found.
  - Returns the correct insertion position if the target is not found.
- **Edge Cases Handled**:
  - Empty array (returns 0).
  - Target smaller than all elements (returns 0).
  - Target larger than all elements (returns `numsSize`).
  - Target equal to an existing element (returns its index).

This solution efficiently meets the problem requirements using binary search, which is optimal for sorted arrays.
