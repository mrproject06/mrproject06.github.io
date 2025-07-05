---
title: "How Pointers work when passed to function"
date: 2025-07-05 00:00:00  +0500
categories: [Pointer-Exercises]
tags: [C]
---

```
Explain this code in layman term with tabular visual representation
#include <stdio.h>

int a = 5, b =10;

void change1(int *p)
{
    p =&a;
}

void change2(int **pp)
{
    *pp=&b;
}



int main(void) {
    int x = 20, *ptr=&x;
    printf("%d ", *ptr);
    change1(ptr);
    printf("%d ", *ptr);
    change2(&ptr);
    printf("%d\n",*ptr);
    return 0;
}
```

This code demonstrates how **pointers** work when passed to functions. It shows:
1. **`change1`** tries (and fails) to modify a pointer.
2. **`change2`** successfully modifies a pointer using a **pointer-to-pointer (`**pp`)**.  

Here’s what happens step-by-step.

---

### **Tabular Representation:**

#### **Initial Setup (Before `main` runs):**

| Variable | Value  | Address (Example) | Notes                     |
|----------|--------|-------------------|---------------------------|
| `a`      | `5`    | `0x1000`          | Global variable           |
| `b`      | `10`   | `0x1004`          | Global variable           |
| `x`      | `20`   | `0x2000`          | Local variable in `main`  |
| `ptr`    | `&x`   | `0x3000`          | Points to `x` (`0x2000`)  |

---

### **Step 1: `printf("%d ", *ptr);`**
- `ptr` points to `x` (value = `20`).
- **Output:** `20`

| Variable | Value  | Points To |
|----------|--------|-----------|
| `ptr`    | `0x2000` | `x` (`20`) |

---

### **Step 2: `change1(ptr);`**
#### **What Happens Inside `change1`:**
```c
void change1(int *p) {
    p = &a; // Changes the **local copy** of `p`, not `ptr` in `main`.
}
```
- `p` gets a copy of `ptr`’s value (`0x2000`).
- `p` is reassigned to `&a` (`0x1000`), but **`ptr` in `main` remains unchanged**.

| Variable (in `change1`) | Value  | Points To |
|-------------------------|--------|-----------|
| `p` (before)            | `0x2000` | `x` (`20`) |
| `p` (after)             | `0x1000` | `a` (`5`) |

#### **Back in `main`:**
- `ptr` **still points to `x` (`20`)**.
- **Output:** `20` (unchanged).

---

### **Step 3: `change2(&ptr);`**
#### **What Happens Inside `change2`:**
```c
void change2(int **pp) {
    *pp = &b; // Modifies `ptr` in `main` to point to `b`.
}
```
- `pp` is a **pointer to `ptr`** (so `*pp` is `ptr` itself).
- `*pp = &b` updates `ptr` to point to `b` (`0x1004`).

| Variable (in `change2`) | Value    | Points To         |
|-------------------------|----------|-------------------|
| `pp`                    | `0x3000` | `ptr` (in `main`) |
| `*pp` (before)          | `0x2000` | `x` (`20`)        |
| `*pp` (after)           | `0x1004` | `b` (`10`)        |

#### **Back in `main`:**
- `ptr` now points to `b` (`10`).
- **Output:** `10`

---

### **Final Output:**
```
20 20 10
```

---

### **Key Takeaways:**
1. **`change1` fails** to modify `ptr` because it receives a **copy** of the pointer.
   - Like giving someone a **duplicate key**—they can’t change your original key.
2. **`change2` succeeds** because it gets the **address of `ptr`** (`&ptr`).
   - Like giving someone a **key to your key drawer**—they can change your keys!
3. **Pointer-to-pointer (`**pp`)** is used when you need to **modify a pointer itself** inside a function.

---

### **Memory Diagram (Simplified):**
```
Global:
a (5) @0x1000    b (10) @0x1004

main():
x (20) @0x2000
ptr @0x3000 → initially points to x (0x2000)
After change2: ptr → b (0x1004)
```

