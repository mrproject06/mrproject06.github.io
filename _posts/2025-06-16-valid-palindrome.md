---
title: "Valid Palindrome"
date: 2025-06-16 00:00:00  +0500
categories: [LeetCode]
tags: [String]
---
**LeetCode Problem** : <https://leetcode.com/problems/valid-palindrome/?envType=problem-list-v2&envId=string>

### Description
A phrase is a palindrome if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string s, return true if it is a palindrome, or false otherwise.

 
### Example
Example 1:

Input: s = "A man, a plan, a canal: Panama"
Output: true
Explanation: "amanaplanacanalpanama" is a palindrome.

Example 2:

Input: s = "race a car"
Output: false
Explanation: "raceacar" is not a palindrome.

## Solution

To determine if a given string is a palindrome, we need to check if it reads the same forwards and backwards after converting all letters to lowercase and removing all non-alphanumeric characters. Here's a step-by-step approach to solve this problem:

### Approach
1. **Normalize the String**:
   - Convert all characters to lowercase.
   - Remove all non-alphanumeric characters (i.e., characters that are not letters or numbers).

2. **Check for Palindrome**:
   - Use two pointers, one starting at the beginning (`left`) and one at the end (`right`) of the normalized string.
   - Move the pointers towards each other, comparing the characters at each position.
   - If any characters do not match, return `false`.
   - If the pointers meet or cross each other without mismatches, return `true`.

### Solution Code
```c
#include <ctype.h>
#include <stdbool.h>
#include <string.h>

bool isPalindrome(char* s) {
    int left = 0;
    int right = strlen(s) - 1;

    while (left < right) {
        // Skip non-alphanumeric characters from the left
        while (left < right && !isalnum(s[left])) {
            left++;
        }
        // Skip non-alphanumeric characters from the right
        while (left < right && !isalnum(s[right])) {
            right--;
        }
        // Compare characters (case-insensitive)
        if (tolower(s[left]) != tolower(s[right])) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

### Explanation
1. **Initialization**:
   - `left` starts at the beginning of the string (`0`).
   - `right` starts at the end of the string (`strlen(s) - 1`).

2. **Skipping Non-Alphanumeric Characters**:
   - Move `left` to the right until an alphanumeric character is found.
   - Move `right` to the left until an alphanumeric character is found.

3. **Character Comparison**:
   - Compare the characters at `left` and `right` (converted to lowercase).
   - If they don't match, return `false`.
   - If they match, move both pointers towards the center (`left++`, `right--`).

4. **Termination**:
   - If the pointers meet or cross each other without any mismatches, return `true`.

### Visual Timeline with Example Walkthrough

**Example**: `s = "A man, a plan, a canal: Panama"`

| Step | `left` | `right` | `s[left]` | `s[right]` | Action                                      | Result So Far |
|------|--------|---------|-----------|------------|---------------------------------------------|---------------|
| 1    | 0      | 16      | 'A'       | 'a'        | Compare 'a' == 'a' → match, move pointers  | Continue       |
| 2    | 1      | 15      | ' '       | 'm'        | Skip non-alphanumeric (`left++`)            | Continue       |
| 3    | 2      | 15      | 'm'       | 'm'        | Compare 'm' == 'm' → match, move pointers  | Continue       |
| 4    | 3      | 14      | 'a'       | 'a'        | Compare 'a' == 'a' → match, move pointers  | Continue       |
| 5    | 4      | 13      | 'n'       | 'n'        | Compare 'n' == 'n' → match, move pointers  | Continue       |
| 6    | 5      | 12      | ','       | 'a'        | Skip non-alphanumeric (`left++`)            | Continue       |
| 7    | 6      | 12      | ' '       | 'a'        | Skip non-alphanumeric (`left++`)            | Continue       |
| 8    | 7      | 12      | 'a'       | 'a'        | Compare 'a' == 'a' → match, move pointers  | Continue       |
| 9    | 8      | 11      | 'p'       | 'l'        | Compare 'p' == 'l' → mismatch → return `false` | False         |

**Final Result**: `false` (The string is not a palindrome after normalization).

### Key Observations
- **Efficiency**: The algorithm runs in \(O(n)\) time, where \(n\) is the length of the string, as each character is processed at most twice (once by each pointer).
- **Space Complexity**: The space complexity is \(O(1)\) as no additional space is used beyond the input string.
- **Edge Cases**: Handles strings with mixed cases, non-alphanumeric characters, and empty strings correctly.

This approach ensures that the string is checked for palindrome properties efficiently and accurately.
