---
title: "Find the index of the first occurrence in a string"
date: 2025-06-08 00:00:00  +0500
categories: [LeetCode]
tags: [String]
---
**LeetCode Problem** : <https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/description/?envType=problem-list-v2&envId=string>

### Description
Given two strings needle and haystack, return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.


### Example
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.

Input: haystack = "leetcode", needle = "leeto"
Output: -1
Explanation: "leeto" did not occur in "leetcode", so we return -1.

### Solution: Use a Stack-Based Approach
To solve this problem, we need to find the first occurrence of the string `needle` in the string `haystack`. If `needle` is found, we return the starting index of its first occurrence; otherwise, we return `-1`.

### Approach
1. **Edge Cases Handling**:
   - If `needle` is an empty string, it should be considered to be present at index `0` of any `haystack`.
   - If the length of `needle` is greater than the length of `haystack`, `needle` cannot be a substring of `haystack`, so we return `-1`.

2. **Sliding Window Comparison**:
   - Iterate through each possible starting position of `needle` in `haystack`.
   - For each starting position, compare the substring of `haystack` starting at that position with `needle`.
   - If a match is found, return the starting index immediately.
   - If no match is found after all iterations, return `-1`.

### Solution Code
```c
#include <string.h>

int strStr(char* haystack, char* needle) {
    int hlen = strlen(haystack);
    int nlen = strlen(needle);

    // Handle edge cases
    if (nlen == 0) return 0;
    if (hlen < nlen) return -1;

    for (int i = 0; i <= hlen - nlen; i++) {
        int j;
        for (j = 0; j < nlen; j++) {
            if (haystack[i + j] != needle[j]) {
                break;
            }
        }
        if (j == nlen) {
            return i;
        }
    }
    return -1;
}
```

### Explanation
1. **Edge Cases Handling**:
   - The function first checks if `needle` is an empty string, returning `0` immediately if true.
   - It then checks if `needle` is longer than `haystack`, returning `-1` if true since `needle` cannot be a substring in this case.

2. **Sliding Window Comparison**:
   - The outer loop runs from `i = 0` to `i = hlen - nlen`, where `i` is the starting index in `haystack` for the current comparison.
   - The inner loop compares each character of `needle` with the corresponding character in `haystack` starting at index `i`.
   - If all characters of `needle` match the substring of `haystack` starting at `i`, the inner loop completes without breaking, and `i` is returned as the starting index.
   - If any character mismatch occurs, the inner loop breaks, and the outer loop increments `i` to try the next starting position.
### Visual Timeline for `haystack = "hello"`, `needle = "ll"`

| Outer Loop | `i` before | Inner Loop (`j`) | Action | `i` after |
|------------|------------|------------------|--------|-----------|
| 1st        | 0          | `j=0`            | Compare `'h'` (haystack[0]) vs `'l'` (needle[0]) → mismatch → break inner loop | 0 |
|            | 0          | -                | Outer loop `i++` | 1 |
| 2nd        | 1          | `j=0`            | Compare `'e'` (haystack[1]) vs `'l'` (needle[0]) → mismatch → break inner loop | 1 |
|            | 1          | -                | Outer loop `i++` | 2 |
| 3rd        | 2          | `j=0`            | Compare `'l'` (haystack[2]) vs `'l'` (needle[0]) → match | 2 |
|            | 2          | `j=1`            | Compare `'l'` (haystack[3]) vs `'l'` (needle[1]) → match | 2 |
|            | 2          | -                | Full match (`j == needlelen`) → return `i = 2` | - |

### Explanation
1. **Outer Loop**:
   - `i` starts at `0` and increments until `i <= hstacklen - needlelen` (`3` for `"hello"` and `"ll"`).
2. **Inner Loop**:
   - For each `i`, `j` iterates from `0` to `needlelen - 1` to compare `haystack[i + j]` with `needle[j]`.
   - If any mismatch occurs, the inner loop breaks, and `i` increments.
3. **Match Found**:
   - At `i = 2`, both characters of `"ll"` match `haystack[2]` and `haystack[3]` → return `2`.

### Key Observations
1. **Efficiency**: The algorithm correctly handles early termination when a mismatch occurs.
2. **Edge Cases**:
   - Empty `needle` → returns `0`.
   - `needle` longer than `haystack` → returns `-1`.
3. **Correctness**: Ensures accurate comparison and proper handling of mismatches.

3. **Return Result**:
   - If the outer loop completes without finding a match, the function returns `-1`, indicating `needle` is not a substring of `haystack`.

This approach efficiently checks all possible starting positions of `needle` in `haystack` and ensures correctness by handling all edge cases appropriately. The time complexity is \(O(n \cdot m)\) in the worst case, where \(n\) is the length of `haystack` and \(m\) is the length of `needle`. The space complexity is \(O(1)\) as no additional space is used beyond the input strings.



