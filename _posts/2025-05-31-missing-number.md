---
title: "Missing Number"
date: 2025-05-31 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/missing-number/description/?envType=problem-list-v2&envId=array>

### Description
Given an array nums containing n distinct numbers in the range [0, n], return the only number in the range that is missing from the array.

### Example
Input: nums = [3,0,1]

Output: 2

Explanation:

n = 3 since there are 3 numbers, so all numbers are in the range [0,3]. 2 is the missing number in the range since it does not appear in nums.

### Solution Code
To find the missing number in an array containing `n` distinct numbers from the range `[0, n]`, we can use a mathematical approach that leverages the properties of the sum of the first `n` natural numbers. Here's a step-by-step explanation and the corresponding solution:

### Approach
1. **Sum of First n Natural Numbers**: The sum of the first `n` natural numbers is given by the formula `sum = n * (n + 1) / 2`. This sum includes all numbers from `0` to `n`.
2. **Calculate the Sum of the Array**: Compute the sum of all elements present in the given array.
3. **Find the Missing Number**: The missing number is the difference between the sum of the first `n` natural numbers and the sum of the array elements.

### Solution Code
```c
#include <stdio.h>

int missingNumber(int* nums, int numsSize) {
    int expectedSum = numsSize * (numsSize + 1) / 2;
    int actualSum = 0;

    for (int i = 0; i < numsSize; i++) {
        actualSum += nums[i];
    }

    return expectedSum - actualSum;
}

int main() {
    int nums1[] = {3, 0, 1};
    int size1 = sizeof(nums1) / sizeof(nums1[0]);
    printf("Missing number: %d\n", missingNumber(nums1, size1)); // Output: 2

    int nums2[] = {0, 1};
    int size2 = sizeof(nums2) / sizeof(nums2[0]);
    printf("Missing number: %d\n", missingNumber(nums2, size2)); // Output: 2

    int nums3[] = {9, 6, 4, 2, 3, 5, 7, 0, 1};
    int size3 = sizeof(nums3) / sizeof(nums3[0]);
    printf("Missing number: %d\n", missingNumber(nums3, size3)); // Output: 8

    return 0;
}
```

### Explanation
1. **Calculate Expected Sum**: The variable `expectedSum` is computed using the formula `n * (n + 1) / 2`, where `n` is the length of the array. This gives the sum of all numbers from `0` to `n`.
2. **Calculate Actual Sum**: The variable `actualSum` is the sum of all elements in the given array. This is computed by iterating through the array and accumulating the values.
3. **Determine Missing Number**: The missing number is found by subtracting `actualSum` from `expectedSum`. This works because the missing number is the only value not accounted for in `actualSum`.

### Example Walkthrough
- **Example 1**: `nums = [3, 0, 1]`
  - `expectedSum = 3 * (3 + 1) / 2 = 6`
  - `actualSum = 3 + 0 + 1 = 4`
  - Missing number = `6 - 4 = 2`

- **Example 2**: `nums = [0, 1]`
  - `expectedSum = 2 * (2 + 1) / 2 = 3`
  - `actualSum = 0 + 1 = 1`
  - Missing number = `3 - 1 = 2`

- **Example 3**: `nums = [9, 6, 4, 2, 3, 5, 7, 0, 1]`
  - `expectedSum = 9 * (9 + 1) / 2 = 45`
  - `actualSum = 9 + 6 + 4 + 2 + 3 + 5 + 7 + 0 + 1 = 37`
  - Missing number = `45 - 37 = 8`

This approach efficiently finds the missing number in **O(n)** time with **O(1)** space complexity, making it optimal for the given problem constraints.
