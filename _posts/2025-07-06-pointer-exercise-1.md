---
title: "Code to print all elements of 2D array"
date: 2025-07-05 00:00:00  +0500
categories: [Pointer-Exercises]
tags: [C]
---

```
#include <stdio.h>

int main(void) {
    int i, arr[3][4]={ {10,11,12,13}, {20,21,22,23}, {30,31,32,33}};
    int *p=&arr[0][0];
    for (i=0; i<12; i++)
    {
        printf("%d ", p[i]);
    }
    printf("\n");
    return 0;
}
```
### **Layman's Explanation with Table & Visualization**

This code prints all elements of a **3x4 grid (2D array)** as if it were a **single row (1D array)**. Here's how it works:

---

### **1. The 2D Array (`arr`)**
Imagine a **3-row × 4-column grid** filled with numbers:

| Row \ Column | 0 | 1  | 2  | 3  |
|--------------|---|----|----|----|
| **Row 0**    | 10| 11 | 12 | 13 |
| **Row 1**    | 20| 21 | 22 | 23 |
| **Row 2**    | 30| 31 | 32 | 33 |

---

### **2. Pointer Setup (`int *p = &arr[0][0]`)**
- `p` is a **pointer** pointing to the **first element (`10`)** of the grid.  
- Even though `arr` is 2D, **in memory, it’s stored as a single row**:  
  ```
  [10][11][12][13][20][21][22][23][30][31][32][33]
  ```
- `p[i]` treats this block as a **1D array** and accesses elements sequentially.

---

### **3. The Loop (`for i=0 to 11`)**
- `p[i]` accesses the `i-th` element in the **flattened 1D array**.  
- **Example Steps:**


| `i` | `p[i]` | Value | Notes                     |
|-----|--------|-------|---------------------------|
| 0   | `p[0]` | 10    | 1st element (`arr[0][0]`) |
| 1   | `p[1]` | 11    | `arr[0][1]`               |
| ... | ...    | ...   | ...                       |
| 4   | `p[4]` | 20    | Start of Row 1 (`arr[1][0]`) |
| 8   | `p[8]` | 30    | Start of Row 2 (`arr[2][0]`) |
| 11  | `p[11]`| 33    | Last element (`arr[2][3]`) |

---

### **4. Output**
The loop prints all 12 numbers in order:  
```
10 11 12 13 20 21 22 23 30 31 32 33
```

---

### **Key Takeaways**
1. **2D arrays are stored linearly in memory** (row-by-row).  
2. A **single pointer (`int *p`)** can traverse the entire grid like a 1D array.  
3. `p[i]` is equivalent to:  
   - Row `i / 4` (since 4 columns exist)  
   - Column `i % 4`  

---

### **ASCII Memory Layout**
```
Pointer `p` starts here:
      ↓
[10][11][12][13][20][21][22][23][30][31][32][33]
 0   1   2   3   4   5   6   7   8   9  10  11  ← Index (`p[i]`)
```

---

### **When Would You Use This?**
- When you need to **process all elements** of a 2D array without nested loops.  
- In image processing (pixels are stored linearly).  

Need a real-world analogy? Think of it like **reading a book page-by-page (2D) vs. scanning the entire book as one long tape (1D)**. 😊  

**Want to see how nested loops compare?** Let me know!
