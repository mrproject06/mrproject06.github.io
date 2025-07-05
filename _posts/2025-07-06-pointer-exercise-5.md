---
title: "Pointer arithmetic to fetch specific value from an array"
date: 2025-07-05 00:00:00  +0500
categories: [Pointer-Exercises]
tags: [C]
---

```
Explain this code in layman term with tabular visual representation.
#include <stdio.h>

int main(void) {
    int x, arr[8]={11,22,33,44,55,66,77,88};
    x = (arr+2)[3];
    printf("%d\n", x);
    return 0;
}
```


This code fetches a specific value from an array using **pointer arithmetic** and prints it. Here's how it works:

1. **An array `arr`** is created with 8 numbers: `[11, 22, 33, 44, 55, 66, 77, 88]`.
2. **`(arr + 2)[3]`** means:
   - Start at the **3rd element (`arr[2] = 33`)**.
   - Move **3 positions ahead** from there → `arr[2 + 3] = arr[5] = 66`.
3. The value **`66`** is stored in `x` and printed.

---

### **Tabular Representation:**

#### **Initial Array Setup:**
| Index (`i`) | Value (`arr[i]`) | Memory Address (Example) |
|------------|------------------|--------------------------|
| 0          | 11               | `1000`                   |
| 1          | 22               | `1004`                   |
| 2          | **33**           | `1008`                   |
| 3          | 44               | `1012`                   |
| 4          | 55               | `1016`                   |
| 5          | **66**           | `1020`                   |
| 6          | 77               | `1024`                   |
| 7          | 88               | `1028`                   |

---

### **Step-by-Step Breakdown:**
1. **`arr + 2`**
   - `arr` points to the start of the array (`arr[0] = 11`).
   - `+ 2` moves the pointer **2 positions ahead** → Now points to `arr[2] = 33`.

2. **`(arr + 2)[3]`**
   - This is equivalent to `arr[2 + 3]` → `arr[5]`.
   - So, it fetches the value at **index 5** → `66`.

3. **Final Assignment**
   - `x = 66` and is printed.

---

### **Pointer Arithmetic Visualization:**
```
arr (base) → [11][22][33][44][55][66][77][88]
               ↑   ↑   ↑   ↑   ↑   ↑   ↑   ↑
Index:        0   1   2   3   4   5   6   7

Step 1: `arr + 2` → Points to `33` (index 2).
Step 2: `(arr + 2)[3]` → Moves 3 steps ahead → `66` (index 5).
```

---

### **Final Output:**
```
66
```

### **Key Takeaways:**
1. **`arr + n`** moves the pointer `n` steps forward in the array.
2. **`(arr + n)[m]`** is equivalent to `arr[n + m]`.
3. The code effectively **accesses `arr[5]`** and prints `66`.

