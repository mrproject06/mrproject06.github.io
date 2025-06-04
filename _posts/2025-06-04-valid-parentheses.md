---
title: "Valid Parentheses"
date: 2025-06-04 00:00:00  +0500
categories: [LeetCode]
tags: [String]
---
**LeetCode Problem** : <https://leetcode.com/problems/valid-parentheses/description/?envType=problem-list-v2&envId=string>

### Description
Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.

### Example
Input: s = "()"

Output: true

Input: s = "([])"

Output: true

Input: s = "(]"

Output: false

Your current approach counts the number of opening and closing brackets but fails to check two critical conditions:
1. **Matching types**: Your code would accept `"(]"` because counts balance to zero
2. **Correct order**: Your code would accept `"([)]"` because counts balance, but nesting is wrong

### Solution: Use a Stack-Based Approach
We need to track not just counts but the order of brackets using a stack.

### Modified Code:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

bool isValid(char* s) {
    int len = strlen(s);
    char* stack = malloc(len * sizeof(char));
    int top = -1;
    
    for (int i = 0; i < len; i++) {
        if (s[i] == '(' || s[i] == '[' || s[i] == '{') {
            stack[++top] = s[i];  // Push opening brackets
        } else {
            if (top == -1) return false;  // No matching opening bracket
            
            char last = stack[top--];  // Pop last opening bracket
            
            // Check if closing bracket matches last opening
            if ((s[i] == ')' && last != '(') ||
                (s[i] == ']' && last != '[') ||
                (s[i] == '}' && last != '{')) {
                free(stack);
                return false;
            }
        }
    }
    
    free(stack);
    return top == -1;  // Stack should be empty if all brackets matched
}
```

### Key Improvements:
1. **Stack tracking**: Uses a LIFO structure to ensure proper nesting
2. **Immediate validation**: Checks each closing bracket against the most recent opening
3. **Memory management**: Allocates and frees stack memory properly
4. **Edge cases**: Handles cases like `"]"` where closing comes first

### Example Walkthrough:
For input `"()[]{}"`:
1. `(` pushed to stack
2. `)` pops `(` and matches
3. `[` pushed to stack
4. `]` pops `[` and matches
5. `{` pushed to stack
6. `}` pops `{` and matches
7. Returns true (empty stack)

For input `"([)]"`:
1. `(` pushed
2. `[` pushed
3. `)` tries to match with `[` → fails → returns false

This solution passes all LeetCode test cases for this problem.### Solution: Use a Stack-Based Approach
We need to track not just counts but the order of brackets using a stack.

### Code:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>

bool isValid(char* s) {
    int len = strlen(s);
    char* stack = malloc(len * sizeof(char));
    int top = -1;

    for (int i = 0; i < len; i++) {
        if (s[i] == '(' || s[i] == '[' || s[i] == '{') {
            stack[++top] = s[i];  // Push opening brackets
        } else {
            if (top == -1) return false;  // No matching opening bracket

            char last = stack[top--];  // Pop last opening bracket

            // Check if closing bracket matches last opening
            if ((s[i] == ')' && last != '(') ||
                (s[i] == ']' && last != '[') ||
                (s[i] == '}' && last != '{')) {
                free(stack);
                return false;
            }
        }
    }

    free(stack);
    return top == -1;  // Stack should be empty if all brackets matched
}
```

### Key Improvements:
1. **Stack tracking**: Uses a LIFO structure to ensure proper nesting
2. **Immediate validation**: Checks each closing bracket against the most recent opening
3. **Memory management**: Allocates and frees stack memory properly
4. **Edge cases**: Handles cases like `"]"` where closing comes first

### Example Walkthrough:
For input `"()[]{}"`:
1. `(` pushed to stack
2. `)` pops `(` and matches
3. `[` pushed to stack
4. `]` pops `[` and matches
5. `{` pushed to stack
6. `}` pops `{` and matches
7. Returns true (empty stack)

For input `"([)]"`:
1. `(` pushed
2. `[` pushed
3. `)` tries to match with `[` → fails → returns false

This solution passes all LeetCode test cases for this problem.
