---
title: "Code to print memory address of each element in an array"
date: 2025-07-05 00:00:00  +0500
categories: [Pointer-Exercises]
tags: [C]
---

```
Explain this code in layman term with tabular visual representation.
#include <stdio.h>

int main(void) {
    int arr[10]={25,30,35,40,55,60,65,70,85,90}, *p;
    for (p = &arr[0]; p<arr+10; p++) {
        printf("%d", p);
    }
    return 0;
}
```


This code prints the **memory addresses** (not the values) of each element in an array of 10 numbers. Here's how it works:

1. **An array `arr`** is created with 10 numbers: `[25, 30, 35, 40, 55, 60, 65, 70, 85, 90]`.
2. **A pointer `p`** is used to **loop through each element** of the array.
3. **Instead of printing the values**, the code prints the **memory locations (`p`)** where these numbers are stored.

⚠️ **Problem:**
- The code uses `%d` (which is for integers) to print memory addresses, which is incorrect.
- **Correct way:** Use `%p` to print pointers (memory addresses).

---

### **Tabular Representation:**

#### **Initial Setup:**
- `arr` contains `[25, 30, 35, 40, 55, 60, 65, 70, 85, 90]`.
- `p` starts at `&arr[0]` (address of the first element).

| Index | Value (`arr`) | Pointer `p` (memory address) | Printed (incorrectly as `%d`) |
|-------|---------------|-----------------------------|-------------------------------|
| 0     | 25            | `p = &arr[0]` (e.g., `1000`) | `1000` (but should be `%p`)   |
| 1     | 30            | `p++` → `&arr[1]` (`1004`)   | `1004`                        |
| 2     | 35            | `p++` → `&arr[2]` (`1008`)   | `1008`                        |
| 3     | 40            | `p++` → `&arr[3]` (`1012`)   | `1012`                        |
| 4     | 55            | `p++` → `&arr[4]` (`1016`)   | `1016`                        |
| 5     | 60            | `p++` → `&arr[5]` (`1020`)   | `1020`                        |
| 6     | 65            | `p++` → `&arr[6]` (`1024`)   | `1024`                        |
| 7     | 70            | `p++` → `&arr[7]` (`1028`)   | `1028`                        |
| 8     | 85            | `p++` → `&arr[8]` (`1032`)   | `1032`                        |
| 9     | 90            | `p++` → `&arr[9]` (`1036`)   | `1036`                        |

---

### **How the Loop Works:**
1. `p = &arr[0]` → Points to the **first element (`25`)**.
2. `p < arr + 10` → Loop runs **10 times** (until the end of the array).
3. `p++` → Moves to the **next memory location** (since `int` is usually 4 bytes, addresses increase by 4).

---

### **Expected Output (Incorrect Format):**
```
1000 1004 1008 1012 1016 1020 1024 1028 1032 1036
```
*(Note: Actual addresses will vary; these are just examples.)*

---

### **Corrected Code (Using `%p` for Addresses):**
```c
#include <stdio.h>

int main(void) {
    int arr[10] = {25, 30, 35, 40, 55, 60, 65, 70, 85, 90}, *p;
    for (p = &arr[0]; p < arr + 10; p++) {
        printf("%p ", (void *)p); // Correct way to print addresses
    }
    return 0;
}
```

### **Key Takeaways:**
1. **Pointers store memory addresses**, not values.
2. **`%d` is wrong for printing addresses** → **Use `%p`**.
3. **`p++` moves to the next memory location** (usually +4 bytes for `int`).
4. The loop runs **10 times**, printing each address in the array.

