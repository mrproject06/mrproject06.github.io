---
title: "Use of pointer to modify values"
date: 2025-07-05 00:00:00  +0500
categories: [Pointer-Exercises]
tags: [C]
---

```
Explain this code in layman term with tabular visual representation.
#include <stdio.h>

int main(void) {
    int i, arr[5]={25,40,55,70,85}, *p=arr;
    for (i = 0; i<8; i++) {
        printf("%d ", ++*p);
    }
    printf("\n");
    for (i = 0; i<7; i++) {
        printf("%d ", (*p)++);
    }
    printf("\n");

    return 0;
}
```

This code uses an array and a pointer to modify and print values in two different ways:
1. **First Loop (`++*p`):**
   - **Increments the value** where `p` points **before** printing it.
   - Runs **8 times** (even though the array has only 5 elements).
   - Since `p` doesn't move, it keeps modifying the **first element (`arr[0]`)**.

2. **Second Loop (`(*p)++`):**
   - **Prints the current value**, then increments it.
   - Runs **7 times**, still modifying `arr[0]`.

⚠️ **Problem:**
- The loops run **more times than the array size**, but `p` never moves.
- This means we keep modifying `arr[0]` repeatedly, **not accessing other elements**.

---

### **Tabular Representation:**

#### **Initial Setup:**
- `arr` contains `[25, 40, 55, 70, 85]`.
- `p` points to `arr[0]` (value = `25`).

| Index | Value (`arr`) | Pointer `p` |
|-------|---------------|-------------|
| 0     | **25**        | → `p` here  |
| 1     | 40            |             |
| 2     | 55            |             |
| 3     | 70            |             |
| 4     | 85            |             |

---

### **First Loop (`++*p`): Increment Before Printing**
- **Action:** `++*p` → **Increase `arr[0]` by 1, then print it.**
- **Runs 8 times** (but only `arr[0]` changes).

| Loop Step (`i`) | Action (`++*p`) | New `arr[0]` | Printed Value | Output So Far |
|----------------|----------------|--------------|---------------|---------------|
| 0              | `++25` → `26`  | **26**       | 26            | 26            |
| 1              | `++26` → `27`  | **27**       | 27            | 26 27         |
| 2              | `++27` → `28`  | **28**       | 28            | 26 27 28      |
| 3              | `++28` → `29`  | **29**       | 29            | 26 27 28 29   |
| 4              | `++29` → `30`  | **30**       | 30            | 26 27 28 29 30 |
| 5              | `++30` → `31`  | **31**       | 31            | ... 31        |
| 6              | `++31` → `32`  | **32**       | 32            | ... 32        |
| 7              | `++32` → `33`  | **33**       | 33            | ... 33        |

**Final `arr` after first loop:** `[33, 40, 55, 70, 85]`
**First Loop Output:** `26 27 28 29 30 31 32 33`

---

### **Second Loop (`(*p)++`): Print Then Increment**
- **Action:** `(*p)++` → **Print `arr[0]`, then increase it by 1.**
- **Runs 7 times** (still modifying `arr[0]`).

| Loop Step (`i`) | Printed Value | Action (`(*p)++`) | New `arr[0]` | Output So Far |
|----------------|---------------|-------------------|--------------|---------------|
| 0              | 33            | `33` → `34`       | **34**       | 33            |
| 1              | 34            | `34` → `35`       | **35**       | 33 34         |
| 2              | 35            | `35` → `36`       | **36**       | 33 34 35      |
| 3              | 36            | `36` → `37`       | **37**       | 33 34 35 36   |
| 4              | 37            | `37` → `38`       | **38**       | ... 37        |
| 5              | 38            | `38` → `39`       | **39**       | ... 38        |
| 6              | 39            | `39` → `40`       | **40**       | ... 39        |

**Final `arr` after second loop:** `[40, 40, 55, 70, 85]`
**Second Loop Output:** `33 34 35 36 37 38 39`

---

### **Final Output:**
```
26 27 28 29 30 31 32 33
33 34 35 36 37 38 39
```

### **Key Takeaways:**
1. `++*p` → **First increments, then prints.**
2. `(*p)++` → **First prints, then increments.**
3. **`p` never moves**, so only `arr[0]` is modified repeatedly.
4. **The loops run more times than array size**, but since `p` stays at `arr[0]`, no out-of-bounds error occurs (but it's risky!).

