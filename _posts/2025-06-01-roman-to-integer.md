---
title: "Roman to Integer"
date: 2025-06-01 00:00:00  +0500
categories: [LeetCode]
tags: [String]
---
**LeetCode Problem** : <https://leetcode.com/problems/roman-to-integer/description/?envType=problem-list-v2&envId=string>

### Description
Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

```text
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```
For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9.
X can be placed before L (50) and C (100) to make 40 and 90.
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

### Example
Input: s = "III"
Output: 3
Explanation: III = 3.

### Solution Code
To convert a Roman numeral to an integer, we need to understand the rules of Roman numerals, including the additive and subtractive principles. Here's a step-by-step approach to solve the problem:

### Approach
1. **Create a Mapping**: Map each Roman symbol to its corresponding integer value.
2. **Iterate Through the String**: Traverse the Roman numeral string from left to right.
3. **Add or Subtract Values**:
   - If the current symbol's value is less than the next symbol's value, subtract the current value from the total (subtractive case).
   - Otherwise, add the current value to the total (additive case).

### Solution Code
```c
#include <stdio.h>
#include <string.h>

int romanToInt(char* s) {
    int values[26]; // Using ASCII values to map to integers
    values['I' - 'A'] = 1;
    values['V' - 'A'] = 5;
    values['X' - 'A'] = 10;
    values['L' - 'A'] = 50;
    values['C' - 'A'] = 100;
    values['D' - 'A'] = 500;
    values['M' - 'A'] = 1000;

    int total = 0;
    int length = strlen(s);

    for (int i = 0; i < length; i++) {
        int current = values[s[i] - 'A'];
        if (i < length - 1) {
            int next = values[s[i + 1] - 'A'];
            if (current < next) {
                total -= current;
            } else {
                total += current;
            }
        } else {
            total += current;
        }
    }

    return total;
}

int main() {
    char* roman1 = "III";
    printf("%s -> %d\n", roman1, romanToInt(roman1)); // Output: 3

    char* roman2 = "LVIII";
    printf("%s -> %d\n", roman2, romanToInt(roman2)); // Output: 58

    char* roman3 = "MCMXCIV";
    printf("%s -> %d\n", roman3, romanToInt(roman3)); // Output: 1994

    return 0;
}
```

### Explanation
1. **Mapping Roman Symbols**: We use an array `values` where the index corresponds to the ASCII value of the Roman symbol (e.g., `'I' - 'A'` gives the index for 'I'). This allows quick lookup of each symbol's value.
2. **Traversing the String**: We iterate through each character in the string.
3. **Checking Subtractive Cases**: For each character, we compare its value with the next character's value. If the current value is less than the next value, we subtract the current value from the total (e.g., `IV` is 4, `IX` is 9).
4. **Adding Values**: If the current value is not less than the next value, we simply add the current value to the total.
5. **Edge Handling**: The last character in the string is always added to the total since there is no next character to compare.

### Example Walkthrough
- **Input**: `"MCMXCIV"`
  - `M` (1000) > `C` (100) → Add 1000 (total = 1000)
  - `C` (100) < `M` (1000) → Subtract 100 (total = 900)
  - `M` (1000) > `X` (10) → Add 1000 (total = 1900)
  - `X` (10) < `C` (100) → Subtract 10 (total = 1890)
  - `C` (100) > `I` (1) → Add 100 (total = 1990)
  - `I` (1) < `V` (5) → Subtract 1 (total = 1989)
  - `V` (5) → Add 5 (total = 1994)
  - **Output**: `1994`

This approach efficiently handles both additive and subtractive cases, ensuring accurate conversion from Roman numerals to integers.

In the provided solution, `'A'` is used as a base to map Roman numeral characters to indices in the `values` array. Here's why `'A'` was chosen and how this mapping works:

### Why `'A'`?
1. **ASCII Values**:
   - Each character in C is represented by its ASCII value.
   - The uppercase letters `'A'` to `'Z'` have consecutive ASCII values:
     - `'A'` = 65, `'B'` = 66, ..., `'Z'` = 90.
   - The Roman numeral symbols (`I`, `V`, `X`, `L`, `C`, `D`, `M`) are all uppercase letters within this range.

2. **Index Calculation**:
   - To map a Roman numeral character to an index in the `values` array, we subtract `'A'` from the character's ASCII value.
     - Example: `'I' - 'A'` = `73 - 65` = `8` (where `'I'` is ASCII 73 and `'A'` is ASCII 65).
   - This gives a unique index for each Roman numeral symbol within the bounds of the `values` array (size 26, covering `'A'` to `'Z'`).

3. **Efficiency**:
   - This method avoids using a `switch` statement or a large lookup table.
   - It directly computes the index for any Roman numeral character in constant time (`O(1)`).

### Example Mapping
| Roman Symbol | ASCII Value | `symbol - 'A'` | Index in `values` |
|--------------|-------------|----------------|-------------------|
| `'I'`        | 73          | `73 - 65`       | 8                 |
| `'V'`        | 86          | `86 - 65`       | 21                |
| `'X'`        | 88          | `88 - 65`       | 23                |
| `'L'`        | 76          | `76 - 65`       | 11                |
| `'C'`        | 67          | `67 - 65`       | 2                 |
| `'D'`        | 68          | `68 - 65`       | 3                 |
| `'M'`        | 77          | `77 - 65`       | 12                |

