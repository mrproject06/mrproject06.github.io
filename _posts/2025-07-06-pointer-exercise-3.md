---
title: "Critical Pointer mistake"
date: 2025-07-05 00:00:00  +0500
categories: [Pointer-Exercises]
tags: [C]
---

```
#include <stdio.h>

void func(int **pp) {
    int num = 10;
    *pp = &num;
}

int main(void) {
    int *ptr;
    func(&ptr);
    printf("%d\n", *ptr);
    return 0;
}
```


This code demonstrates a **critical pointer mistake** that leads to **undefined behavior**. Here's what happens:

1. **`func`** tries to make `ptr` point to a **local variable (`num`)** inside `func`.
2. **Problem:** `num` is destroyed when `func` ends, so `ptr` becomes a **dangling pointer** (points to invalid memory).
3. **`printf`** tries to read `*ptr`, but the value is **garbage** (or crashes).

---

### **Tabular Representation:**

#### **Step 1: Before `func` is called**

| Variable | Value  | Notes                     |
|----------|--------|---------------------------|
| `ptr`    | `?`    | Uninitialized (garbage)   |

---

#### **Step 2: Inside `func`**

| Variable | Value  | Notes                      |
|----------|--------|----------------------------|
| `num`    | `10`   | Local to `func`            |
| `pp`     | `&ptr` | Pointer-to-pointer         |
| `*pp`    | `&num` | `ptr` now points to `num`  |

**Memory Layout in `func`:**
```
ptr → num (10) [valid here]
```

---

#### **Step 3: After `func` ends**
- `num` is **destroyed** (stack memory freed).
- `ptr` still points to where `num` **used to be** (now invalid).

| Variable | Value  | Notes                     |
|----------|--------|---------------------------|
| `ptr`    | `&num` | **Dangling pointer!**     |

**Memory Layout after `func`:**
```
ptr → [garbage / undefined]
```

---

#### **Step 4: `printf("%d\n", *ptr);`**
- Tries to read `*ptr` (invalid memory).
- **Undefined behavior:** Might print garbage, crash, or behave unpredictably.

---

### **Key Takeaways:**
1. **Never return pointers to local variables.**
   - Local variables die when the function ends.
2. **Dangling pointers** are dangerous.
   - They point to memory that no longer holds the expected value.
3. **Undefined behavior** means the code might "work" sometimes but fail unpredictably.

---

### **Visualization of the Mistake:**
```
func():
   num = 10 (address: 0x1000)
   ptr → 0x1000 [while func is running]

After func():
   num is destroyed (0x1000 is now invalid)
   ptr → 0x1000 [DANGLING!]
```

---

### **How to Fix It:**
- Allocate `num` on the heap (using `malloc`) if you need it to persist.
- Or pass a valid memory address (e.g., from `main`).

#### **Fixed Code (Using Heap):**
```c
#include <stdio.h>
#include <stdlib.h>

void func(int **pp) {
    int *num = malloc(sizeof(int)); // Lives until freed
    *num = 10;
    *pp = num; // Safe: heap memory persists
}

int main(void) {
    int *ptr;
    func(&ptr);
    printf("%d\n", *ptr); // Output: 10
    free(ptr); // Don't forget to free!
    return 0;
}
```

---

### **Final Output (Original Code):**
```
[Garbage value] or [Crash]
```
*(Depends on compiler/platform, but **never reliable**.)*

