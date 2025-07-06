---
title: "Difference Between char *name_ptr and char name_array[] in C"
date: 2025-07-06 00:00:00  +0500
categories: [Tech-Jargons]
tags: [C]
---
### **Difference Between `char *name_ptr` and `char name_array[]` in C**

Both represent strings, but they work differently in memory. Here's a simple breakdown:

---

### **1. `char *name_ptr` (Pointer to String)**
- **What it is**: A **pointer** that holds the address of a string (stored in read-only or heap memory).
- **Memory Allocation**: The string itself may be in:
  - **Read-only memory** (if pointing to a string literal).
  - **Heap memory** (if allocated with `malloc`).
- **Modifiable?** ✅ **Pointer can change**, but ❌ **string content may be read-only**.
- **Example**:
  ```c
  char *name_ptr = "Alice";  // Stored in read-only memory (may crash if modified)
  name_ptr[0] = 'B';         // ❌ Undefined behavior (trying to modify read-only memory)
  name_ptr = "Bob";          // ✅ OK (pointer can point to a new string)
  ```

---

### **2. `char name_array[]` (Array of Characters)**
- **What it is**: A **fixed-size array** that stores the string directly in stack memory.
- **Memory Allocation**: The string is stored in **writable stack memory**.
- **Modifiable?** ✅ **Content can be changed**, but ❌ **array size is fixed**.
- **Example**:
  ```c
  char name_array[] = "Alice";  // Stored in stack memory (modifiable)
  name_array[0] = 'B';         // ✅ OK (modifies to "Blice")
  // name_array = "Bob";        // ❌ ERROR (can't reassign an array)
  ```

---

### **Key Differences**

| Feature                | `char *name_ptr`               | `char name_array[]`            |
|------------------------|-------------------------------|--------------------------------|
| **Memory Location**    | Heap/Read-only                | Stack                          |
| **Size Flexibility**   | Can point to any string       | Fixed size (cannot be resized) |
| **Modifiable Content?**| ❌ (If string literal)        | ✅ (Always modifiable)         |
| **Reassignable?**      | ✅ (Pointer can change)       | ❌ (Array is fixed)            |

---

### **Example Code (Demonstrating Both)**
```c
#include <stdio.h>
#include <string.h>

int main() {
    // Case 1: Pointer to string literal (READ-ONLY)
    char *name_ptr = "Alice";
    printf("Pointer: %s\n", name_ptr);  // Output: Alice
    // name_ptr[0] = 'B';  // ❌ CRASH (trying to modify read-only memory)
    name_ptr = "Bob";      // ✅ OK (pointer reassignment)
    printf("Pointer now: %s\n", name_ptr);  // Output: Bob

    // Case 2: Array (MODIFIABLE)
    char name_array[] = "Alice";
    printf("Array: %s\n", name_array);  // Output: Alice
    name_array[0] = 'B';   // ✅ OK (modifies to "Blice")
    printf("Array now: %s\n", name_array);  // Output: Blice
    // name_array = "Bob";  // ❌ ERROR (cannot reassign array)

    return 0;
}
```

---

### **When to Use Which?**
- **Use `char *name_ptr`**:
  - When you need to **reassign** the pointer.
  - When working with **dynamic strings** (`malloc`, `strdup`).
- **Use `char name_array[]`**:
  - When you need a **modifiable fixed-size string**.
  - When working with **local buffers** (e.g., user input).

---

### **Summary**
- `char *ptr` → **Flexible pointer**, but may point to read-only memory.
- `char arr[]` → **Modifiable array**, but fixed in size.

Choose based on whether you need **reassignment** (`*ptr`) or **modifiability** (`arr[]`). 🚀
