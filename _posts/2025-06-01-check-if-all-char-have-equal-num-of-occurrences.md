---
title: "Check if All characters Have Equal Number of Occurrences"
date: 2025-06-01 00:00:00  +0500
categories: [LeetCode]
tags: [String]
---
**LeetCode Problem** : <https://leetcode.com/problems/check-if-all-characters-have-equal-number-of-occurrences/description/?envType=problem-list-v2&envId=string>

### Description
Given a string s, return true if s is a good string, or false otherwise.

A string s is good if all the characters that appear in s have the same number of occurrences (i.e., the same frequency).

### Example
Input: s = "abacbc"
Output: true
Explanation: The characters that appear in s are 'a', 'b', and 'c'. All characters occur 2 times in s.

### Solution Code
To determine if a string is "good" (i.e., all characters in the string appear with the same frequency), we can follow these steps:

### Approach
1. **Count Character Frequencies**: First, we'll count how many times each character appears in the string.
2. **Check Uniform Frequency**: Next, we'll check if all these counts (frequencies) are the same. If they are, the string is good; otherwise, it's not.

### Solution Code
```c
#include <stdio.h>
#include <stdbool.h>
#include <string.h>

#define NUM_CHARS 256 // Assuming ASCII characters

bool isGoodString(const char* s) {
    int freq[NUM_CHARS] = {0};
    int len = strlen(s);

    // Count the frequency of each character
    for (int i = 0; i < len; i++) {
        freq[(unsigned char)s[i]]++;
    }

    // Find the first non-zero frequency to compare others against
    int firstFreq = 0;
    for (int i = 0; i < NUM_CHARS; i++) {
        if (freq[i] != 0) {
            firstFreq = freq[i];
            break;
        }
    }

    // Check all non-zero frequencies against the first one
    for (int i = 0; i < NUM_CHARS; i++) {
        if (freq[i] != 0 && freq[i] != firstFreq) {
            return false;
        }
    }

    return true;
}

int main() {
    const char* s1 = "aabbcc";
    printf("%s: %s\n", s1, isGoodString(s1) ? "true" : "false"); // true

    const char* s2 = "aabbccc";
    printf("%s: %s\n", s2, isGoodString(s2) ? "true" : "false"); // false

    const char* s3 = "abcabc";
    printf("%s: %s\n", s3, isGoodString(s3) ? "true" : "false"); // true

    const char* s4 = "a";
    printf("%s: %s\n", s4, isGoodString(s4) ? "true" : "false"); // true

    return 0;
}
```

### Explanation
1. **Frequency Counting**: We initialize an array `freq` of size 256 (to cover all ASCII characters) with zeros. We then iterate through each character in the string, incrementing the corresponding index in `freq` for each character encountered.
2. **Finding Reference Frequency**: We scan the `freq` array to find the first non-zero frequency. This frequency will be our reference to check against all other non-zero frequencies.
3. **Uniformity Check**: We iterate through the `freq` array again. If any non-zero frequency differs from the reference frequency, we immediately return `false`. If all non-zero frequencies match the reference, we return `true`.

This approach efficiently checks for uniform character frequencies in O(n) time, where n is the length of the string, and uses O(1) additional space (since the frequency array size is fixed).

### Dry Run of Code for `"abacbc"`:
1. Count frequencies:
   - `'a'`: 2, `'b'`: 2, `'c'`: 2.
2. First non-zero frequency: `2`.
3. Check all non-zero frequencies:
   - `'a'`: 2 == 2 ✔️
   - `'b'`: 2 == 2 ✔️
   - `'c'`: 2 == 2 ✔️
4. Return `true`.

### Key Takeaways:
- The XOR method is unreliable for this problem because it fails when the number of distinct characters with the same frequency is odd.
- The correct approach is to count frequencies and explicitly check if they all match the first non-zero frequency.
- Time complexity: O(n) (counting) + O(256) (checking frequencies) = O(n).
- Space complexity: O(256) = O(1) (fixed-size frequency array).
