---
title: "Insertion Sort"
date: 2025-08-24 00:00:00  +0500
categories: [Sorting]
tags: [C]
---

## **🧠 Layman's Explanation: The School Class Photo**

**Imagine:** You're arranging students for a class photo by height.

- **Quick Sort Approach**: Pick one student (the "pivot"), say a medium-height student. Then:
  1. Have all shorter students stand to the left
  2. Have all taller students stand to the right  
  3. Now do the same for the left group and right group separately

- **Insertion Sort Approach**: Arrange students one by one, moving each new student left until they're in the right position (like inserting cards into a sorted hand)

---

## **📝 Step-by-Step Quick Sort Example**

Let's sort: `[38, 27, 43, 3, 9, 82, 10]`

We'll use the **last element as pivot** for simplicity.

### **Step 1: Initial Array**
```
[38, 27, 43, 3, 9, 82, 10]
Pivot: 10 (last element)
```

### **Step 2: Partitioning**
Move elements so that:
- All elements < 10 go left
- All elements > 10 go right

**After partitioning:**
```
[3, 9, 10, 43, 38, 82, 27]
```
Pivot (10) is now in its correct position!

### **Step 3: Recursively Sort Left and Right**
**Left of pivot:** `[3, 9]` (already sorted)  
**Right of pivot:** `[43, 38, 82, 27]`

### **Step 4: Sort Right Side `[43, 38, 82, 27]`**
```
Pivot: 27 (last element)
Partition: [27, 38, 82, 43] → 27 in correct position
Left: [] (empty)
Right: [38, 82, 43]
```

### **Step 5: Sort `[38, 82, 43]`**
```
Pivot: 43 (last element)  
Partition: [38, 43, 82] → 43 in correct position
Left: [38] (sorted)
Right: [82] (sorted)
```

### **Final Sorted Array:**
```
[3, 9, 10, 27, 38, 43, 82]
```

---

## **👨‍💻 Simple C Implementation**

```c
#include <stdio.h>

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high];  // Choose last element as pivot
    int i = (low - 1);      // Index of smaller element
    
    for (int j = low; j <= high - 1; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }
    swap(&arr[i + 1], &arr[high]);
    return (i + 1);
}

void quickSort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);  // pi is pivot index
        
        quickSort(arr, low, pi - 1);   // Sort elements before pivot
        quickSort(arr, pi + 1, high);  // Sort elements after pivot
    }
}

int main() {
    int arr[] = {38, 27, 43, 3, 9, 82, 10};
    int n = sizeof(arr)/sizeof(arr[0]);
    
    printf("Original: ");
    for (int i = 0; i < n; i++) printf("%d ", arr[i]);
    
    quickSort(arr, 0, n-1);
    
    printf("\nSorted:   ");
    for (int i = 0; i < n; i++) printf("%d ", arr[i]);
    
    return 0;
}
```

**Output:**
```
Original: 38 27 43 3 9 82 10 
Sorted:   3 9 10 27 38 43 82 
```

---

## **🔍 How Partitioning Works (Step-by-Step)**

**Partitioning `[38, 27, 43, 3, 9, 82, 10]` with pivot=10:**

```
Step 0: [38, 27, 43, 3, 9, 82, 10]  i=-1, j=0
Step 1: j=0: 38 < 10? No            → no swap
Step 2: j=1: 27 < 10? No            → no swap  
Step 3: j=2: 43 < 10? No            → no swap
Step 4: j=3: 3 < 10? Yes → i=0, swap 38↔3 → [3, 27, 43, 38, 9, 82, 10]
Step 5: j=4: 9 < 10? Yes → i=1, swap 27↔9 → [3, 9, 43, 38, 27, 82, 10]
Step 6: j=5: 82 < 10? No            → no swap
Step 7: Swap pivot: swap 43↔10 → [3, 9, 10, 38, 27, 82, 43]
```

Pivot 10 is now at index 2 (correct position)!

---

## **⚡ Quick Sort vs Insertion Sort: Key Differences**

### **Quick Sort (Divide and Conquer)**
- **Time Complexity**: 
  - Average: O(n log n)
  - Worst: O(n²) - but rare with good pivot selection
- **Space Complexity**: O(log n) - recursive calls
- **Best for**: Large datasets, general-purpose sorting

### **Insertion Sort (Incremental)**
- **Time Complexity**:
  - Average: O(n²)
  - Best: O(n) - for nearly sorted arrays
- **Space Complexity**: O(1) - in-place
- **Best for**: Small datasets, nearly sorted data

---

## **📊 Performance Comparison**

| Scenario | Quick Sort | Insertion Sort |
|----------|------------|----------------|
| **Large random data** | ⚡ Very Fast | 🐌 Very Slow |
| **Nearly sorted data** | 🐌 Can be slow | ⚡ Very Fast |
| **Small datasets** | 🐌 Overhead | ⚡ Fast |
| **Memory usage** | Moderate (recursion) | Excellent (in-place) |

---

## **🎯 When to Use Which**

### **Use Quick Sort When:**
- Sorting large datasets (>1000 elements)
- Data is randomly ordered
- Average performance matters most

### **Use Insertion Sort When:**
- Sorting small datasets (<100 elements)
- Data is already nearly sorted
- Memory is very limited
- Implementing for educational purposes

---

## **⚡ Why Quick Sort is Usually Better**

1. **O(n log n) vs O(n²)**: For large n, n log n is much faster than n²
   - n=1000: 1000×10 = 10,000 vs 1000×1000 = 1,000,000
   - n=1,000,000: 1M×20 = 20M vs 1M×1M = 1,000,000M

2. **Cache Friendly**: Quick Sort has good locality of reference

3. **In-place**: Doesn't require extra memory like Merge Sort

## **⚠️ Quick Sort's Weakness**
- Worst-case O(n²) if poor pivot choices (already sorted/reverse sorted)
- Not stable (equal elements may change order)
- Recursive (stack overflow risk for huge arrays)

In practice, Quick Sort is often the default choice for general-purpose sorting because it's usually the fastest!
