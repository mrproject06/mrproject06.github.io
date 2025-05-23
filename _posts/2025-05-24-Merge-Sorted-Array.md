---
title: "Merge Sorted Array"
date: 2025-05-24 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/merge-sorted-array/description/?envType=problem-list-v2&envId=array>

### Description
You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n.

### Example
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

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
