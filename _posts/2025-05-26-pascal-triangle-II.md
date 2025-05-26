---
title: "Pascal Triangle II"
date: 2025-05-26 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/pascals-triangle-ii/description/?envType=problem-list-v2&envId=array>

### Description

Given an integer rowIndex, return the rowIndexth (0-indexed) row of the Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

### Example

Input: rowIndex = 3
Output: [1,3,3,1]

To retrieve a specific row from Pascal's Triangle efficiently, we can use a mathematical approach that avoids generating the entire triangle. Here's a clear C solution with explanation:

### Solution Code
```c
#include <stdio.h>
#include <stdlib.h>

int* getRow(int rowIndex, int* returnSize) {
    *returnSize = rowIndex + 1;
    int* row = (int*)malloc((rowIndex + 1) * sizeof(int));
    row[0] = 1; // First element is always 1

    for (int i = 1; i <= rowIndex; i++) {
        // Compute using the previous value to avoid factorial calculations
        row[i] = (long)row[i - 1] * (rowIndex - i + 1) / i;
    }

    return row;
}

int main() {
    int rowIndex = 3;
    int returnSize;
    int* row = getRow(rowIndex, &returnSize);

    printf("Row %d: [", rowIndex);
    for (int i = 0; i < returnSize; i++) {
        printf("%d", row[i]);
        if (i < returnSize - 1) printf(", ");
    }
    printf("]\n");

    free(row);
    return 0;
}
```

### Explanation

1. **Key Insight**:
   - Each element in Pascal's Triangle can be calculated using combinations: C(n, k) = n!/(k!(n-k)!)
   - We optimize this using the relation: C(n, k) = C(n, k-1) × (n - k + 1)/k

2. **Implementation**:
   - Initialize the first element as 1 (all rows start with 1)
   - For each subsequent position:
     - Calculate based on previous value to avoid factorial computation
     - Use `long` to prevent integer overflow during multiplication
     - The formula: `current = previous × (rowIndex - position + 1) / position`

3. **Example** (rowIndex = 3):
   - Start: [1, 0, 0, 0]
   - Step 1: [1, 1×3/1=3, 0, 0]
   - Step 2: [1, 3, 3×2/2=3, 0]
   - Step 3: [1, 3, 3, 3×1/3=1]
   - Result: [1, 3, 3, 1]

4. **Advantages**:
   - Time Complexity: O(n) - Single pass through the row
   - Space Complexity: O(n) - Only stores the target row
   - No recursion or full triangle generation needed

### Edge Cases Handled
- rowIndex = 0 → Returns [1]
- rowIndex = 1 → Returns [1, 1]
- Large rowIndex (up to 33) → Handles with long casting to prevent overflow

This approach efficiently computes any specific row in Pascal's Triangle using optimal mathematical operations.
