---
title: "Longest Common Prefix"
date: 2025-06-02 00:00:00  +0500
categories: [LeetCode]
tags: [String]
---
**LeetCode Problem** : <https://leetcode.com/problems/longest-common-prefix/description/?envType=problem-list-v2&envId=string>

### Description
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

### Example
Example 1:

Input: strs = ["flower","flow","flight"]
Output: "fl"
Example 2:

Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.

### Solution Code
To find the longest common prefix among an array of strings, we can follow a systematic approach where we compare each character of the first string with the corresponding characters of all other strings. Here's a step-by-step explanation and the corresponding solution:

### Approach
1. **Handle Edge Cases**: If the input array is empty, return an empty string.
2. **Initialize the Prefix**: Start with the first string as the initial prefix.
3. **Iterate Through Strings**: For each subsequent string in the array, compare its characters with the current prefix.
4. **Update the Prefix**: For each string, adjust the prefix to only include characters that match up to the current length of the prefix.
5. **Terminate Early**: If at any point the prefix becomes empty, return immediately as no common prefix exists.

### Solution Code
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char* longestCommonPrefix(char** strs, int strsSize) {
    if (strsSize == 0) {
        char* result = (char*)malloc(1);
        result[0] = '\0';
        return result;
    }

    // Start with the first string as the initial prefix
    char* prefix = strs[0];
    int prefixLen = strlen(prefix);

    for (int i = 1; i < strsSize; i++) {
        int j = 0;
        while (j < prefixLen && j < strlen(strs[i]) && prefix[j] == strs[i][j]) {
            j++;
        }
        prefixLen = j;
        if (prefixLen == 0) {
            break;
        }
    }

    // Allocate memory for the result and copy the prefix
    char* result = (char*)malloc(prefixLen + 1);
    if (result == NULL) {
        return NULL;
    }
    strncpy(result, prefix, prefixLen);
    result[prefixLen] = '\0';

    return result;
}

int main() {
    char* strs1[] = {"flower", "flow", "flight"};
    int size1 = sizeof(strs1) / sizeof(strs1[0]);
    char* result1 = longestCommonPrefix(strs1, size1);
    printf("Longest common prefix: %s\n", result1); // Output: "fl"
    free(result1);

    char* strs2[] = {"dog", "racecar", "car"};
    int size2 = sizeof(strs2) / sizeof(strs2[0]);
    char* result2 = longestCommonPrefix(strs2, size2);
    printf("Longest common prefix: %s\n", result2); // Output: ""
    free(result2);

    return 0;
}
```

### Explanation
1. **Edge Case Handling**: If the input array `strs` is empty, the function returns an empty string.
2. **Initial Prefix**: The first string in the array is taken as the initial prefix.
3. **String Comparison**: For each subsequent string, the function compares each character with the current prefix. The loop continues until a mismatch is found or the end of the current string or prefix is reached.
4. **Prefix Adjustment**: The length of the prefix is adjusted to the point where all compared strings have matching characters.
5. **Result Construction**: The function allocates memory for the result, copies the determined prefix into it, and ensures the string is null-terminated.
6. **Memory Management**: The result is dynamically allocated, so the caller must free the memory after use to prevent leaks.

This approach efficiently finds the longest common prefix by leveraging character-by-character comparison and dynamically adjusting the prefix length based on the matches found. The time complexity is O(S), where S is the sum of all characters in all strings, and the space complexity is O(1) additional space (excluding the space for the result).

Let's walk through the example step by step to understand how the `longestCommonPrefix` function works.

### Example Walkthrough

**Input:** `strs = ["flower", "flow", "flight"]`
**Expected Output:** `"fl"`

#### Step-by-Step Execution:

1. **Initialization**:
   - `strsSize = 3` (number of strings in the array).
   - `prefix = "flower"` (first string in the array).
   - `prefixLen = 6` (length of `"flower"`).

2. **First Comparison (`prefix` vs `"flow"`)**:
   - Compare each character of `prefix` (`"flower"`) with `"flow"`:
     - `'f' == 'f'` → Continue.
     - `'l' == 'l'` → Continue.
     - `'o' == 'o'` → Continue.
     - `'w' == 'w'` → Continue.
     - `'e' != ' '` (end of `"flow"` is reached).
   - Update `prefixLen` to `4` (length of `"flow"`).
   - New `prefix` (implicitly): `"flow"` (but we only track `prefixLen`).

3. **Second Comparison (`prefix` vs `"flight"`)**:
   - Compare each character of `prefix` (`"flow"`, up to `prefixLen = 4`) with `"flight"`:
     - `'f' == 'f'` → Continue.
     - `'l' == 'l'` → Continue.
     - `'o' != 'i'` → Mismatch found.
   - Update `prefixLen` to `2` (position where mismatch occurs).
   - New `prefix` (implicitly): `"fl"`.

4. **Termination**:
   - No more strings to compare (`i = 3` exceeds `strsSize - 1`).
   - Allocate memory for the result and copy `"fl"` (first `prefixLen = 2` characters of `prefix`).
   - Return `"fl"`.

---

**Another Example:**
**Input:** `strs = ["dog", "racecar", "car"]`
**Expected Output:** `""` (no common prefix)

#### Step-by-Step Execution:

1. **Initialization**:
   - `strsSize = 3`.
   - `prefix = "dog"`.
   - `prefixLen = 3`.

2. **First Comparison (`prefix` vs `"racecar"`)**:
   - Compare `'d'` vs `'r'` → Mismatch immediately.
   - Update `prefixLen = 0`.
   - Break the loop early (no need to check further).

3. **Result**:
   - Allocate memory for an empty string (`""`).
   - Return `""`.

---

### Key Observations:
- The algorithm efficiently narrows down the prefix by comparing each string with the current prefix.
- It stops early if the prefix becomes empty, optimizing performance.
- The memory for the result is dynamically allocated and must be freed by the caller.

### Edge Cases Handled:
1. **Empty Input Array**:
   - Returns `""`.
2. **Single String in Array**:
   - Returns the string itself as the prefix.
3. **No Common Prefix**:
   - Returns `""` (e.g., `["dog", "racecar", "car"]`).

This approach ensures correctness and efficiency with a time complexity of **O(S)** (where S is the total number of characters in all strings) and space complexity of **O(1)** (excluding the result storage).

In C, `char** strs` is a **pointer to a pointer to a character**, which is commonly used to represent an array of strings (or an array of character arrays). Here's a detailed breakdown of its role in the `longestCommonPrefix` function:

---

### **Role of `char** strs`**
1. **Represents an Array of Strings**:
   - `char** strs` is equivalent to `char* strs[]`, which is an array where each element is a `char*` (a string).
   - Example:
     ```c
     char* strs[] = {"flower", "flow", "flight"}; // Array of 3 strings
     ```
     Here, `strs` is an array of pointers, where each pointer (`strs[0]`, `strs[1]`, `strs[2]`) points to the first character of a string.

2. **Passing to the Function**:
   - When calling `longestCommonPrefix(strs, 3)`, the array `strs` decays into a pointer to its first element (`char**`).
   - Thus, `strs` inside the function is a pointer to the first string (`"flower"`), and `strs + 1` points to `"flow"`, etc.

3. **Accessing Strings**:
   - `strs[i]` gives the `i-th` string in the array.
     Example:
     - `strs[0]` → `"flower"` (type `char*`).
     - `strs[1]` → `"flow"`.
     - `strs[2]` → `"flight"`.

4. **Why Not `char* strs`?**:
   - `char*` would represent a single string, not an array of strings.
   - `char**` is necessary to handle multiple strings dynamically.

---

### **How It Works in `longestCommonPrefix`**
1. **Initialization**:
   - The function starts with the first string (`strs[0]`) as the initial prefix.

2. **Iteration**:
   - The loop compares each subsequent string (`strs[1]`, `strs[2]`, etc.) with the current prefix.
   - For each string `strs[i]`, it checks how many characters match the prefix.

3. **Example**:
   - For `strs = ["flower", "flow", "flight"]`:
     - Compare `"flower"` (prefix) with `"flow"` → prefix becomes `"flow"`.
     - Compare `"flow"` with `"flight"` → prefix becomes `"fl"`.

4. **Termination**:
   - If the prefix length reduces to `0`, the function returns `""`.

---

### **Key Points**
- **Memory Layout**:
  - `strs` points to an array of `char*` (each `char*` points to a string).
  - Example memory structure:
    ```
    strs[0] → "flower\0"
    strs[1] → "flow\0"
    strs[2] → "flight\0"
    ```

- **Why Double Pointer?**:
  - Allows the function to work with a dynamically sized array of strings.
  - Essential for handling input arrays like `["a", "b", "c"]` or `["hello", "world"]`.

- **Edge Cases**:
  - If `strsSize = 0`, the function returns `""`.
  - If `strsSize = 1`, the function returns the only string as the prefix.

---

### **Example Code Snippet**
```c
char* longestCommonPrefix(char** strs, int strsSize) {
    if (strsSize == 0) return ""; // Handle empty input
    char* prefix = strs[0];       // Start with the first string
    for (int i = 1; i < strsSize; i++) {
        // Compare prefix with strs[i] and update prefix length
    }
    return prefix;
}
```

---

### **Summary**
- `char** strs` is a **pointer to an array of strings**.
- It enables the function to:
  - Iterate through all input strings.
  - Dynamically adjust the common prefix.
  - Handle variable-sized input arrays efficiently.

This approach ensures the function works correctly for any number of input strings while maintaining clarity and efficiency.
