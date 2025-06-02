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

Here's a simple and efficient C solution to remove all occurrences of a given value from an array in-place:

### Solution Code
```c
int removeElement(int* nums, int numsSize, int val) {
    int k = 0;  // Initialize the counter for non-val elements
    
    for (int i = 0; i < numsSize; i++) {
        if (nums[i] != val) {
            nums[k] = nums[i];  // Move non-val element to position k
            k++;                // Increment count of non-val elements
        }
    }
    
    return k;  // Return the count of elements not equal to val
}
```

### Explanation

1. **Initialization**:
   - `k` is initialized to 0 to keep track of the position where the next non-`val` element should be placed.

2. **Iteration**:
   - Loop through each element in the array using index `i`.
   - For each element, check if it is not equal to `val`.

3. **In-Place Modification**:
   - If the element is not equal to `val`, it is moved to the position `k` in the array.
   - `k` is then incremented to point to the next position for a non-`val` element.

4. **Result**:
   - After processing all elements, `k` will contain the count of elements that are not equal to `val`.
   - The first `k` elements of the array will contain all the non-`val` elements, and the order of these elements is preserved (though the order of the remaining elements is not important).

### Example Usage

```c
#include <stdio.h>

int main() {
    int nums[] = {3, 2, 2, 3};
    int val = 3;
    int size = sizeof(nums) / sizeof(nums[0]);
    
    int k = removeElement(nums, size, val);
    
    printf("Number of elements not equal to %d: %d\n", val, k);
    printf("Modified array: ");
    for (int i = 0; i < k; i++) {
        printf("%d ", nums[i]);
    }
    
    return 0;
}
```

### Output
For the input array `{3, 2, 2, 3}` and `val = 3`, the output will be:
```
Number of elements not equal to 3: 2
Modified array: 2 2
```

### Key Features
- **Time Complexity**: O(n) - The algorithm makes a single pass through the array.
- **Space Complexity**: O(1) - The algorithm uses constant extra space.
- **In-Place Operation**: The array is modified in-place without using additional storage.
- **Order Preservation**: The relative order of the non-`val` elements is preserved, though the order of the remaining elements (if any) is not important.

This solution efficiently meets the problem requirements by ensuring that all non-`val` elements are moved to the front of the array in their original order, while the count of such elements is returned.
