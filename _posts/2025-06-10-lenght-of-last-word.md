---
title: "Lenght of Last Word"
date: 2025-06-10 00:00:00  +0500
categories: [LeetCode]
tags: [String]
---
**LeetCode Problem** : <https://leetcode.com/problems/length-of-last-word/description/?envType=problem-list-v2&envId=string>

### Description
Given a string s consisting of words and spaces, return the length of the last word in the string.

A word is a maximal substring consisting of non-space characters only.

### Example
Example 1:

Input: s = "Hello World"
Output: 5
Explanation: The last word is "World" with length 5.
Example 2:

Input: s = "   fly me   to   the moon  "
Output: 4
Explanation: The last word is "moon" with length 4.

### Solution: Use a Stack-Based Approach
To solve this problem, we need to find the length of the last word in a given string. A word is defined as a maximal substring of non-space characters. 

### Approach
1. **Trim Trailing Spaces**: First, we need to ignore any trailing spaces at the end of the string since they do not contribute to the last word.
2. **Find the Last Word**: Starting from the end of the trimmed string, we move backwards until we encounter a space, which indicates the start of the last word.
3. **Calculate Length**: The length of the last word is the difference between the end of the string and the position where the last space was found.

### Solution Code
```c
#include <string.h>

int lengthOfLastWord(char* s) {
    int length = 0;
    int i = strlen(s) - 1;
    
    // Skip trailing spaces
    while (i >= 0 && s[i] == ' ') {
        i--;
    }
    
    // Count the length of the last word
    while (i >= 0 && s[i] != ' ') {
        length++;
        i--;
    }
    
    return length;
}
```

### Explanation
1. **Initialization**: We start by initializing `length` to `0` and set `i` to the last index of the string.
2. **Trimming Trailing Spaces**: The first `while` loop moves `i` backwards until it finds a non-space character or reaches the beginning of the string. This step ensures we ignore any trailing spaces.
3. **Counting the Last Word**: The second `while` loop counts the number of non-space characters starting from `i` (which is now at the end of the last word) and moving backwards until a space is encountered or the start of the string is reached. The count of these characters gives the length of the last word.
4. **Return Result**: The function returns the computed `length`.

### Visual Timeline with Example Walkthrough
**Example**: `s = "Hello World   "`

1. **Initial String**: `'H', 'e', 'l', 'l', 'o', ' ', 'W', 'o', 'r', 'l', 'd', ' ', ' ', ' '`
   - Length of string: 14
   - Initial `i`: 13 (last index)

2. **Trimming Trailing Spaces**:
   - `i = 13`: `s[13] = ' '` → decrement `i` to 12
   - `i = 12`: `s[12] = ' '` → decrement `i` to 11
   - `i = 11`: `s[11] = ' '` → decrement `i` to 10
   - `i = 10`: `s[10] = 'd'` → stop trimming

3. **Counting Last Word**:
   - `i = 10`: `s[10] = 'd'` → `length = 1`, decrement `i` to 9
   - `i = 9`: `s[9] = 'l'` → `length = 2`, decrement `i` to 8
   - `i = 8`: `s[8] = 'r'` → `length = 3`, decrement `i` to 7
   - `i = 7`: `s[7] = 'o'` → `length = 4`, decrement `i` to 6
   - `i = 6`: `s[6] = 'W'` → `length = 5`, decrement `i` to 5
   - `i = 5`: `s[5] = ' '` → stop counting

4. **Result**: The last word is `"World"` with length `5`.

### Key Observations
- **Efficiency**: The algorithm efficiently processes the string in \(O(n)\) time, where \(n\) is the length of the string, by traversing it from the end to the beginning.
- **Space Complexity**: The space complexity is \(O(1)\) as no additional space is used beyond the input string.
- **Edge Cases**: Handles strings with trailing spaces and ensures correct results even if the string ends with multiple spaces.
