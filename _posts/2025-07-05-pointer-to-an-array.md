---
title: "Pointer to an Array"
date: 2025-07-05 00:00:00  +0500
categories: [Tech-Jargons]
tags: [C]
---

### **📦 Pointer to an Array - Explained Simply**

#### **Imagine a Bookshelf:**
- A **regular array** is like a bookshelf with fixed slots:
  ```
  Shelf: [📕, 📗, 📘, 📙]  // Indexes: 0, 1, 2, 3
  ```
- A **pointer to the array** is like a **bookmark** that can point to the **entire shelf** or move between books.

---

### **🔑 Key Idea**
A **pointer to an array** points to the **whole shelf (array)** at once, not just one book (element).

---

### **🛠️ C Example**
```c
#include <stdio.h>

int main() {
    // 1. Create a bookshelf (array)
    char books[4] = {'📕', '📗', '📘', '📙'};

    // 2. Create a pointer to the ENTIRE array
    char (*shelf_ptr)[4] = &books;  // Points to whole shelf

    // 3. Access books through the pointer
    printf("First book: %c\n", (*shelf_ptr)[0]);  // 📕
    printf("Third book: %c\n", (*shelf_ptr)[2]);  // 📘

    return 0;
}
```

#### **Output:**
```
First book: 📕
Third book: 📘
```

---

### **🎨 ASCII Art Explanation**
```
shelf_ptr (📍) 
    |
    ▼
[ 📕, 📗, 📘, 📙 ]  // Entire array
```
- `(*shelf_ptr)[0]` → 📕 (first book)
- `(*shelf_ptr)[2]` → 📘 (third book)

---

### **🚶‍♂️ How It Works**
1. **`&books`** gives the **address of the whole array** (not just the first element).
2. **`char (*shelf_ptr)[4]`** declares a pointer to an array of 4 chars.
3. **`(*shelf_ptr)[index]`** accesses elements like a normal array.

---

### **🆚 vs Regular Pointer**
| Feature | Pointer to Array | Regular Pointer |
|---------|------------------|-----------------|
| **Declares** | `char (*ptr)[4]` | `char *ptr` |
| **Points to** | Entire array | Single element |
| **Access** | `(*ptr)[0]` | `ptr[0]` or `*ptr` |
| **Pointer Math** | Moves by **whole array size** | Moves by **element size** |

---

### **💡 When to Use?**
- When you need to pass/return **entire arrays** to functions.
- When working with **multi-dimensional arrays** (like grids).


### Book Notes

We can declare a pointer that can point to the whole array instead of only one element of array.

```
int (*ptr)[10];
```

Here ptr is pointer that can point to an array of 10 integers. Since subscripts have higher precedence
than indirection, it is necessary to enclose the indirection operator and pointer name inside parentheses.
Here the type of ptr is `pointer to an array of 10 integers`.

```
int *p ; // can point to an integer
int (*ptr)[5] ; // can point to an array of 5 integers
int arr[5];
p = arr; // points to 0th element of arr
p = &arr; // points to while array arr
```

**Program to dereference a pointer to an array**

Whenever a pointer to an array is dereferenced, we get the base address of the array to which it points.
```
int arr[5] = {3,5,6,7,9};
int *p = arr;
int (*ptr)[5] = &arr;
printf("p =%p, ptr=%p\n",p,ptr);
printf("*p =%d, *ptr=%p\n", *p, *ptr);
printf("sizeof(p)=%u, sizeof(*p)=%u\n", sizeof(p), sizeof(*p));
printf("sizeof(ptr)=%u, sizeof(*ptr)=%u\n", sizeof(ptr), sizeof(*ptr));
```

```
O/P:
p=0012FEC4, ptr=0012FEC4
*p =3, *ptr=0012FEC4
sizeof(p)=4, sizeof(*p)=4
sizeof(ptr)=4, sizeof(*ptr)=20
```

