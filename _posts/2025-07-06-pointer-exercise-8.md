---
title: "Pointer to print elements of an array in 2 different orders"
date: 2025-07-05 00:00:00  +0500
categories: [Pointer-Exercises]
tags: [C]
---

```
Explain this code in layman term with tabular visual representation.
#include <stdio.h>

int main(void) {
    int i, arr[5]={25,40,55,70,85}, *p=arr;
    for (i = 0; i<5; i++) {
        printf("%d ", *p++);
    }
    printf("\n");
    for (i = 0; i<5; i++) {
        printf("%d ", *--p);
    }
    printf("\n");

    return 0;
}
```



Here's a simple explanation of the code along with a tabular representation to help you visualize how it works:

This code creates an array of 5 numbers and uses a pointer to print them in two different orders:
1. **First Loop:** Prints the numbers in their original order (25, 40, 55, 70, 85) while moving the pointer forward.
2. **Second Loop:** Prints the numbers in reverse order (85, 70, 55, 40, 25) by moving the pointer backward.

### **Tabular Representation:**

#### **Initial Setup:**
- `arr` is an array with 5 numbers.
- `p` is a pointer that initially points to the first element of `arr`.

| Index | Value | Pointer `p` position |
|-------|-------|----------------------|
| 0     | 25    | → p points here first |
| 1     | 40    |                      |
| 2     | 55    |                      |
| 3     | 70    |                      |
| 4     | 85    |                      |

---

### **First Loop (`*p++`):**
- Prints the value where `p` is pointing, then moves `p` to the next element.

| Step | `i` | `*p` (printed) | Action (`p++`) | Output So Far |
|------|-----|----------------|----------------|---------------|
| 1    | 0   | 25             | Moves to `arr[1]` | 25            |
| 2    | 1   | 40             | Moves to `arr[2]` | 25 40         |
| 3    | 2   | 55             | Moves to `arr[3]` | 25 40 55      |
| 4    | 3   | 70             | Moves to `arr[4]` | 25 40 55 70   |
| 5    | 4   | 85             | Moves **past** `arr[4]` | 25 40 55 70 85 |

After the first loop, `p` points **one position after `arr[4]`** (out of bounds).

---

### **Second Loop (`*--p`):**
- First **moves `p` backward**, then prints the value it points to.

| Step | `i` | Action (`--p`) | New `p` position | `*p` (printed) | Output So Far |
|------|-----|----------------|------------------|----------------|---------------|
| 1    | 0   | Moves back     | Points to `arr[4]` | 85             | 85            |
| 2    | 1   | Moves back     | Points to `arr[3]` | 70             | 85 70         |
| 3    | 2   | Moves back     | Points to `arr[2]` | 55             | 85 70 55      |
| 4    | 3   | Moves back     | Points to `arr[1]` | 40             | 85 70 55 40   |
| 5    | 4   | Moves back     | Points to `arr[0]` | 25             | 85 70 55 40 25 |

---

### **Final Output:**
```
25 40 55 70 85  
85 70 55 40 25  
```

### **Key Takeaways:**
- `*p++` means **"print the current value, then move forward."**
- `*--p` means **"move backward first, then print the new value."**
- The pointer moves step by step through memory, changing where it points each time.

