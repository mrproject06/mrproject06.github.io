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

Here's a simple and efficient C solution to remove duplicates from a sorted array in-place:

```c
int removeDuplicates(int* nums, int numsSize) {
    if (numsSize == 0) return 0;  // Handle empty array case

    int k = 1;  // Start with 1 since first element is always unique

    for (int i = 1; i < numsSize; i++) {
        // If current element is different from previous unique element
        if (nums[i] != nums[k-1]) {
            nums[k] = nums[i];  // Store the new unique element
            k++;                // Increment unique count
        }
    }

    return k;  // Return number of unique elements
}
```

### How This Works:

1. **Initialization**:
   - `k` starts at 1 because the first element is always unique
   - We begin checking from the second element (`i = 1`)

2. **Iteration**:
   - Compare each element with the last unique element (at position `k-1`)
   - If different, it's a new unique element

3. **In-Place Modification**:
   - Store new unique elements at position `k`
   - Increment `k` to track the count of unique elements

4. **Edge Cases Handled**:
   - Empty array (`numsSize == 0`)
   - Single-element array (automatically returns 1)
   - Array with all identical elements

### Example Usage:

```c
#include <stdio.h>

int main() {
    int nums[] = {0,0,1,1,1,2,2,3,3,4};
    int size = sizeof(nums)/sizeof(nums[0]);

    int k = removeDuplicates(nums, size);

    printf("Unique elements: %d\n", k);
    printf("Modified array: ");
    for (int i = 0; i < k; i++) {
        printf("%d ", nums[i]);
    }

    return 0;
}
```

**Output**:
```
Unique elements: 5
Modified array: 0 1 2 3 4
```

### Key Features:
- **Time Complexity**: O(n) - single pass through the array
- **Space Complexity**: O(1) - no additional storage used
- **Stable**: Maintains original order of unique elements
- **In-Place**: Modifies the input array directly

This solution efficiently solves the problem while meeting all the requirements specified.
