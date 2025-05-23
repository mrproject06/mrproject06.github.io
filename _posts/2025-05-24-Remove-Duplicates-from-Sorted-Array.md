---
title: "Remove Duplicates from Sorted Array"
date: 2025-05-24 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/?envType=problem-list-v2&envId=arrayi>

### Description
Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

Consider the number of unique elements of nums to be k, to get accepted, you need to do the following things:

Change the array nums such that the first k elements of nums contain the unique elements in the order they were present in nums initially. The remaining elements of nums are not important as well as the size of nums.
Return k.


### Example
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

Here's an efficient C solution to merge two sorted arrays into `nums1`:

### Solution Code
```c
void merge(int* nums1, int nums1Size, int m, int* nums2, int nums2Size, int n) {
    int i = m - 1;      // Last element of nums1
    int j = n - 1;      // Last element of nums2
    int k = m + n - 1;  // Last position of merged array
    
    // Merge from the end
    while (i >= 0 && j >= 0) {
        if (nums1[i] > nums2[j]) {
            nums1[k--] = nums1[i--];
        } else {
            nums1[k--] = nums2[j--];
        }
    }
    
    // Copy remaining elements from nums2 if any
    while (j >= 0) {
        nums1[k--] = nums2[j--];
    }
}
```

### Explanation

1. **Three Pointers Technique**:
   - `i` points to the last valid element in `nums1` (index `m-1`)
   - `j` points to the last element in `nums2` (index `n-1`)
   - `k` points to the last position in the merged array (index `m+n-1`)

2. **Backward Merging**:
   - Compare elements from the end of both arrays
   - Place the larger element at position `k`
   - Move the respective pointer backward
   - This avoids overwriting elements in `nums1` that haven't been processed yet

3. **Remaining Elements**:
   - If `nums2` has remaining elements after `nums1` is exhausted, copy them directly
   - No need to handle remaining `nums1` elements as they're already in place

### Example Usage

```c
#include <stdio.h>

int main() {
    int nums1[6] = {1, 3, 5, 0, 0, 0};
    int nums2[3] = {2, 4, 6};
    
    merge(nums1, 6, 3, nums2, 3, 3);
    
    printf("Merged array: ");
    for (int i = 0; i < 6; i++) {
        printf("%d ", nums1[i]);
    }
    
    return 0;
}
```

### Output
For inputs:
- `nums1 = [1, 3, 5, 0, 0, 0]` (m = 3)
- `nums2 = [2, 4, 6]` (n = 3)

Output:
```
Merged array: 1 2 3 4 5 6
```

### Key Features

- **Time Complexity**: O(m + n) - Single pass through both arrays
- **Space Complexity**: O(1) - No additional memory used
- **In-Place Operation**: Modifies `nums1` directly
- **Stable**: Maintains the order of equal elements
- **Edge Cases Handled**:
  - Empty `nums1` or `nums2`
  - All elements in one array being smaller than the other
  - Duplicate values between arrays

This solution efficiently merges the arrays while utilizing the existing space in `nums1`, meeting all the problem requirements.
