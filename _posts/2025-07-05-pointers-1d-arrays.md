---
title: "Pointers and 1-D Array"
date: 2025-07-05 00:00:00  +0500
categories: [Tech-Jargons]
tags: [C]
---

### **📚 Pointers & 1D Arrays Explained Simply**

#### **1. What is an Array?**
An array is like a **row of lockers** - each stores one item, and they're numbered in order.

**ASCII Art:**
```
[ 🧦 ] [ 👟 ] [ 🧢 ] [ 🎒 ]  ← Array of 4 items
  0      1      2      3    ← Index numbers
```

#### **2. What is a Pointer?**
A pointer is like a **signpost** - it doesn't store the item itself, but **points to where the item is stored**.

**ASCII Art:**
```
   ptr → [ 🧦 ]
          (Locker #0)
```

---

### **🧑‍🏫 C Code Example**
```c
#include <stdio.h>

int main() {
    // 1. Create an array (4 lockers)
    char locker[4] = {'🧦', '👟', '🧢', '🎒'};

    // 2. Create a pointer pointing to the array
    char *ptr = locker; // Same as &locker[0]

    // 3. Access elements
    printf("First item: %c\n", *ptr);   // 🧦 (item at locker[0])
    printf("Second item: %c\n", ptr[1]);// 👟 (same as locker[1])

    // 4. Move pointer to next item
    ptr++;
    printf("Now pointing to: %c\n", *ptr); // 👟

    return 0;
}
```

#### **Output:**
```
First item: 🧦
Second item: 👟
Now pointing to: 👟
```

---

### **🔑 Key Concepts**

| Concept | Explanation | Example |
|---------|-------------|---------|
| **Array** | A block of items | `locker[4] = {'🧦','👟','🧢','🎒'}` |
| **Pointer** | Signpost to an item | `char *ptr = locker;` |
| `*ptr` | Gets the pointed item | `*ptr` → `'🧦'` |
| `ptr++` | Moves to next item | Now points to `'👟'` |
| `ptr[1]` | Same as `*(ptr + 1)` | Gets `'👟'` |

---

### **🔄 Pointer vs Array Access**
**Same Result, Different Ways:**
```c
printf("%c", locker[2]);    // 🧢 (array style)
printf("%c", *(ptr + 2));   // 🧢 (pointer style)
```

**ASCII Visualization:**
```
Pointer Math:
ptr + 0 → [ 🧦 ]
ptr + 1 → [ 👟 ]
ptr + 2 → [ 🧢 ]  ← *(ptr + 2) gives 🧢
```

---

### **💡 Why Use Pointers?**
1. **Efficiency:** Directly access memory locations.
2. **Flexibility:** Can move between items (`ptr++`).
3. **Used Everywhere:** Strings, functions, dynamic memory.


### Book Notes
1. Elements of an array are stored in consecutive memory location.
2. When the name of an array is used in any expression, the value of the name is a constant pointer
that denotes the address of the first element of the array(eg. array name arr in an expression is
equivalent to &arr[0]).
3. According to pointer arthmetic, when a pointer variable is incremented, it points to the next
location of its base type.

Let us take an example:
int arr[5] = {5, 10, 15, 20, 25};

Here arr is an array that has 5 elements each of type int.

We can get the address of an element of array by applying & operator in front of subscripted variable
name. Hence &arr[0] gives address of 0th element, &arr[1] gives the address of 1st element and so on.

The name of array arr denotes the address of the 0th element of array. The name of an array is a constant
pointer, and according to pointer arthmetic when an integer is added to a pointer then we get the address
of next element of same base type. Hence arr+1 will denote the address of the next element arr[1].

arr    --> points to 0th element --> &arr[0] --> 2000
arr+1  --> points to 1st element --> &arr[1] --> 2004
arr+2  --> points to 2nd element --> &arr[2] --> 2006

In general, we can write;

The pointer expression arr+i denotes the same address as &arr[i].

Now if we deference arr, then we get the 0th element of array i.e. expression *arr or *(arr+0)
represents 0th element of array.

```
*arr     --> value of 0th element --> arr[0] --> 5
*(arr+1) --> value of 1st element --> arr[1] --> 10
*(arr+2) --> value of 2nd element --> arr[2] --> 15
```
In general, we can write;
```
*(arr+i) --> arr[i]
```

So in subscript notation the address of an arrya element is &arr[i] and its value is arr[i], while
in pointer notation the address is (arr+i) and the value is *(arr+i).

Array subscripting is commutative, i.e. arr[i] is same as i[arr].

**Subscripting Pointer Variables**

Suppose we take a pointer variable ptr, and initialize it with the address of the 0th element an 
array arr.

```
int *ptr;
ptr = arr; // We can also write ptr = &arr[0]
```

Now let see the difference between the name of an array and a pointer variable. Then name of an 
array is a constant pointer and so it will always point to the 0th element of the array. It is not
a variable so we can't assign some other address to it neither can we move it by incrementing or 
decrementing.

```
arr = &num /*Illegal*/
arr++; /*Illegal*/
arr = arr-1; /*Illegal*/
```
But since ptr is a pointer variable, all those operations are valid for it.

```
ptr = &num; /*Now ptr points to a variable num*/
ptr++; /*ptr points to next location*/
ptr = ptr-1; /*ptr points to previous location*/
```

