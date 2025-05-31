---
title: "Majority Element"
date: 2025-05-31 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/majority-element/description/?envType=problem-list-v2&envId=array>

### Description
Given an array nums of size n, return the majority element.

The majority element is the element that appears more than ⌊n / 2⌋ times. You may assume that the majority element always exists in the array.

### Example
Input: nums = [3,2,3]
Output: 3

### Solution Code
To find the majority element in an array (which appears more than ⌊n/2⌋ times), we can use the **Boyer-Moore Voting Algorithm**. This algorithm efficiently finds the majority element in **O(n) time** and **O(1) space**.

### Solution Code
```c
#include <stdio.h>

int majorityElement(int* nums, int numsSize) {
    int candidate = nums[0];
    int count = 1;

    for (int i = 1; i < numsSize; i++) {
        if (count == 0) {
            candidate = nums[i];
            count = 1;
        } else if (nums[i] == candidate) {
            count++;
        } else {
            count--;
        }
    }

    // Since majority element always exists, no need for verification
    return candidate;
}

int main() {
    int nums1[] = {3, 2, 3};
    int size1 = sizeof(nums1) / sizeof(nums1[0]);
    printf("Majority element: %d\n", majorityElement(nums1, size1)); // Output: 3

    int nums2[] = {2, 2, 1, 1, 1, 2, 2};
    int size2 = sizeof(nums2) / sizeof(nums2[0]);
    printf("Majority element: %d\n", majorityElement(nums2, size2)); // Output: 2

    return 0;
}
```

### Explanation (Boyer-Moore Voting Algorithm)

1. **Initialization**:
   - Start with the first element as the `candidate` and `count = 1`.

2. **Traverse the Array**:
   - For each subsequent element:
     - If `count == 0`, set the current element as the new `candidate` and reset `count = 1`.
     - If the current element matches the `candidate`, increment `count`.
     - If the current element differs, decrement `count`.

3. **Result**:
   - The `candidate` at the end is the majority element (since it appears more than ⌊n/2⌋ times).

### Why This Works
- The algorithm cancels out non-majority elements with majority elements.
- Since the majority element appears more than half the time, it will always survive the cancellation process.

### Example Walkthrough
For `nums = [2, 2, 1, 1, 1, 2, 2]`:
- Initialize: `candidate = 2`, `count = 1`
- Next `2`: `count = 2`
- Next `1`: `count = 1`
- Next `1`: `count = 0` → New `candidate = 1`, `count = 1`
- Next `1`: `count = 2`
- Next `2`: `count = 1`
- Next `2`: `count = 0` → New `candidate = 2`, `count = 1`
- Final `candidate = 2` (majority element).

### Key Features
- **Time Complexity**: O(n) (single pass through the array).
- **Space Complexity**: O(1) (uses only two extra variables).
- **Guaranteed Correctness**: Works because the majority element appears more than ⌊n/2⌋ times.

This approach is optimal for the given problem constraints. 🚀
