---
title: "Insertion Sort"
date: 2025-08-24 00:00:00  +0500
categories: [Sorting]
tags: [C]
---

```
#include <stdio.h>

void insertionSort(int arr[], int n) {
    int i, j, current;
    
    printf("Starting with: ");
    for (int k = 0; k < n; k++) printf("%d ", arr[k]);
    printf("\n\n");
    
    for (i = 1; i < n; i++) {
        current = arr[i];
        j = i - 1;
        
        printf("Iteration %d: Inserting %d\n", i, current);
        printf("Before: ");
        for (int k = 0; k < n; k++) printf("%d ", arr[k]);
        printf("\n");
        
        // Find where to insert 'current'
        while (j >= 0 && arr[j] > current) {
            arr[j + 1] = arr[j]; // Shift element right
            j = j - 1;
        }
        
        arr[j + 1] = current; // Insert in correct position
        
        printf("After:  ");
        for (int k = 0; k < n; k++) printf("%d ", arr[k]);
        printf("\n\n");
    }
}

int main() {
    int arr[] = {12, 11, 13, 5, 6};
    int n = sizeof(arr) / sizeof(arr[0]);
    
    insertionSort(arr, n);
    
    printf("Final sorted array: ");
    for (int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    
    return 0;
}

```

```
Starting with: 12 11 13 5 6

Iteration 1: Inserting 11
Before: 12 11 13 5 6
After:  11 12 13 5 6

Iteration 2: Inserting 13
Before: 11 12 13 5 6
After:  11 12 13 5 6

Iteration 3: Inserting 5
Before: 11 12 13 5 6
After:  5 11 12 13 6

Iteration 4: Inserting 6
Before: 5 11 12 13 6
After:  5 6 11 12 13

Final sorted array: 5 6 11 12 13
```
