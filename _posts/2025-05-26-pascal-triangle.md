---
title: "Pascal Triangle"
date: 2025-05-26 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/pascals-triangle/description/?envType=problem-list-v2&envId=array>

### Description

Given an integer numRows, return the first numRows of Pascal's triangle.

In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:

### Example

Input: numRows = 5
Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]

Here's a complete C solution to generate Pascal's Triangle for a given number of rows:

### Solution Code
```c
#include <stdio.h>
#include <stdlib.h>

int** generate(int numRows, int* returnSize, int** returnColumnSizes) {
    // Allocate memory for the triangle and column sizes
    *returnSize = numRows;
    *returnColumnSizes = (int*)malloc(numRows * sizeof(int));
    int** triangle = (int**)malloc(numRows * sizeof(int*));

    for (int i = 0; i < numRows; i++) {
        // Set column size for current row
        (*returnColumnSizes)[i] = i + 1;

        // Allocate memory for current row
        triangle[i] = (int*)malloc((i + 1) * sizeof(int));

        // First and last elements are always 1
        triangle[i][0] = 1;
        triangle[i][i] = 1;

        // Calculate middle elements
        for (int j = 1; j < i; j++) {
            triangle[i][j] = triangle[i-1][j-1] + triangle[i-1][j];
        }
    }

    return triangle;
}

void printTriangle(int** triangle, int numRows, int* columnSizes) {
    for (int i = 0; i < numRows; i++) {
        for (int j = 0; j < columnSizes[i]; j++) {
            printf("%d ", triangle[i][j]);
        }
        printf("\n");
    }
}

int main() {
    int numRows = 5;
    int returnSize;
    int* returnColumnSizes;

    int** triangle = generate(numRows, &returnSize, &returnColumnSizes);

    printf("Pascal's Triangle for %d rows:\n", numRows);
    printTriangle(triangle, numRows, returnColumnSizes);

    // Free memory
    for (int i = 0; i < numRows; i++) {
        free(triangle[i]);
    }
    free(triangle);
    free(returnColumnSizes);

    return 0;
}
```

### Explanation

1. **Memory Allocation**:
   - Allocate memory for the triangle structure (array of arrays)
   - Allocate memory for the column sizes array

2. **Row Construction**:
   - Each row `i` has `i+1` elements
   - First and last elements of each row are always 1
   - Middle elements are the sum of two elements from the previous row

3. **Output Example** (for numRows = 5):
   ```
   1
   1 1
   1 2 1
   1 3 3 1
   1 4 6 4 1
   ```

4. **Memory Management**:
   - Properly allocates memory for the triangle structure
   - Includes cleanup code to free all allocated memory

### Key Features

- **Time Complexity**: O(n²) where n is numRows (we process each element once)
- **Space Complexity**: O(n²) (space needed to store the triangle)
- **Correctness**:
  - Handles edge cases (numRows = 0, 1)
  - Properly implements Pascal's Triangle rules
- **Memory Safety**:
  - Follows the requirement that caller must free memory
  - Includes complete cleanup in the example usage

This solution efficiently generates Pascal's Triangle while properly managing memory allocation and deallocation as specified in the problem constraints.
