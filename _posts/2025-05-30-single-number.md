---
title: "Single Number"
date: 2025-05-38 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/single-number/description/?envType=problem-list-v2&envId=array>

### Description
Given a non-empty array of integers nums, every element appears twice except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

### Example
Input: nums = [2,2,1]

Output: 1

### Solution Code
To find the single element in an array where every other element appears twice, we can use the XOR bitwise operation. This approach is efficient and meets the problem's constraints of linear runtime and constant space complexity.

### Solution Code
```c
#include <stdio.h>

int singleNumber(int* nums, int numsSize) {
    int result = 0;
    for (int i = 0; i < numsSize; i++) {
        result ^= nums[i];
    }
    return result;
}

int main() {
    int nums1[] = {2, 2, 1};
    int size1 = sizeof(nums1)/sizeof(nums1[0]);
    printf("Single number: %d\n", singleNumber(nums1, size1)); // Output: 1

    int nums2[] = {4, 1, 2, 1, 2};
    int size2 = sizeof(nums2)/sizeof(nums2[0]);
    printf("Single number: %d\n", singleNumber(nums2, size2)); // Output: 4

    int nums3[] = {1};
    int size3 = sizeof(nums3)/sizeof(nums3[0]);
    printf("Single number: %d\n", singleNumber(nums3, size3)); // Output: 1

    return 0;
}
```

### Explanation

1. **XOR Property**:
   - XOR of a number with itself is 0: `a ^ a = 0`
   - XOR of a number with 0 is the number itself: `a ^ 0 = a`
   - XOR is commutative and associative: `a ^ b ^ a = (a ^ a) ^ b = 0 ^ b = b`

2. **How It Works**:
   - Initialize `result` to 0
   - Iterate through each number in the array and perform XOR with `result`
   - Pairs of identical numbers will cancel each other out (become 0)
   - The final `result` will be the single number that doesn't have a pair

3. **Example Walkthrough** (for [4,1,2,1,2]):
   - Start: result = 0
   - 0 ^ 4 = 4
   - 4 ^ 1 = 5
   - 5 ^ 2 = 7
   - 7 ^ 1 = 6
   - 6 ^ 2 = 4
   - Final result: 4

### Key Features
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Uses only one extra variable
- **Efficiency**: No additional memory needed beyond the input
- **Correctness**: Works for all valid inputs, including when the single number is at the beginning, middle, or end of the array

This solution efficiently finds the single number by leveraging the properties of the XOR operation, making it both optimal and easy to understand.
