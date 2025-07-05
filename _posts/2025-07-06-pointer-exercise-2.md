---
title: "Pointer behaviour when passed to functions"
date: 2025-07-05 00:00:00  +0500
categories: [Pointer-Exercises]
tags: [C]
---

```
#include <stdio.h>

void func1(int *ptr);
void func2(int **pptr);

int main(void) {
    int arr[5]={1,2,3,4,5};
    int *p =arr;
    printf("p  =%p,\t", p);
    func1(p);
    printf("p =%p,\t", p);
    func2(&p);
    printf("p=%p\n", p);
    return 0;
}

void func1(int *ptr)
{
    ptr++;
}

void func2(int **pptr)
{
    (*pptr)++;
}
```


This code demonstrates how **pointers behave differently** when passed to functions normally (`func1`) vs. using a **pointer-to-pointer** (`func2`). Here's what happens:

---

### **Initial Setup (Before Any Function Calls)**

| Variable | Value | Points To | Notes |
|----------|-------|-----------|-------|
| `arr` | `{1,2,3,4,5}` | `arr[0]` (value `1`) | Array in memory |
| `p` | `&arr[0]` (e.g., `0x1000`) | `arr[0]` (`1`) | Pointer initialized to `arr` |

---

### **Step 1: First `printf` (Before `func1`)**
```c
printf("p  =%p,\t", p);  // Prints address of arr[0] (e.g., 0x1000)
```
**Output:**  
`p  =0x1000,`

---

### **Step 2: Calling `func1(p)`**
#### **What Happens in `func1`:**
```c
void func1(int *ptr) {
    ptr++;  // Moves ptr to next array element (LOCALLY)
}
```

| Variable (in `func1`) | Value Before | Value After | Notes |
|-----------------------|--------------|-------------|-------|
| `ptr` (copy of `p`) | `0x1000` | `0x1004` | **Local change only!** |

#### **Back in `main`:**
- `p` **remains unchanged** (still `0x1000`) because `func1` got a **copy** of `p`.
- **Output after `func1`:**  
  `p =0x1000,`

---

### **Step 3: Calling `func2(&p)`**
#### **What Happens in `func2`:**
```c
void func2(int **pptr) {
    (*pptr)++;  // Moves the ORIGINAL p to next element
}
```
| Variable (in `func2`) | Value Before | Value After | Notes |
|-----------------------|--------------|-------------|-------|
| `pptr` (address of `p`) | `&p` (e.g., `0x2000`) | Same | Points to `p` |
| `*pptr` (original `p`) | `0x1000` | `0x1004` | **Actually modifies `p`!** |

#### **Back in `main`:**
- `p` **now points to `arr[1]`** (`0x1004`) because `func2` modified it directly.
- **Final Output:**  
  `p=0x1004`

---

### **Final Output Example**
```
p  =0x1000,    p =0x1000,    p=0x1004
```

---

### **Key Takeaways (Tabular Summary)**
| Function | How Pointer is Passed | Effect | Why? |
|----------|-----------------------|--------|------|
| `func1(p)` | By value (copy) | **No change** to `p` | Modifies a local copy |
| `func2(&p)` | By reference (`int **`) | **Changes `p`** | Modifies the original pointer |

---

### **Memory Visualization**
```
Before func2:
arr: [1][2][3][4][5]
     0x1000 0x1004 ...
     ^
     p (0x1000)

After func2:
arr: [1][2][3][4][5]
         0x1004 ...
         ^
         p (now 0x1004)
```

---

### **Why This Matters**
1. **`func1` fails** to modify `p` because C passes copies of pointers by default.  
2. **`func2` succeeds** because we pass the **address of `p`** (`&p`), allowing direct modification.  
3. **Pointer-to-pointer (`**`)**: Needed when you want to change where a pointer points *inside a function*.

---

### **Analogy**
- **`func1`**: Giving a friend a **photocopy of your address**. They can scribble on it, but your real address stays the same.  
- **`func2`**: Giving a friend **your address written on a sticky note**. They can update the real thing!  

