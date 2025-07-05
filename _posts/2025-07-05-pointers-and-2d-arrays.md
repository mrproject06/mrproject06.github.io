---
title: "Pointer and 2-D arrays"
date: 2025-07-05 00:00:00  +0500
categories: [Tech-Jargons]
tags: [C]
---

### **📦 Pointers & 2D Arrays Explained Simply**

#### **Imagine a Comic Book Store:**
- A **2D array** is like a store with **shelves (rows)** and **comics (columns)**:
  ```
  Shelf 0: [🦸, 🦇, 🕷️]
  Shelf 1: [👾, 🤖, 👽]
  Shelf 2: [🐉, 🦄, 🦕]
  ```
- A **pointer** is like a **laser pointer** that can highlight any shelf or comic.

---

### **🔑 Key Concepts**
1. **2D Array**: Grid of values (like the comic store).
2. **Pointer to Array**: Can point to a **whole shelf (row)** or the **entire store**.

---

### **🛠️ C Examples**

#### **1. Pointer to a Row (Shelf)**
```c
#include <stdio.h>

int main() {
    char comics[3][3] = {
        {'🦸', '🦇', '🕷️'},
        {'👾', '🤖', '👽'},
        {'🐉', '🦄', '🦕'}
    };

    // Pointer to Shelf 1 (a single row)
    char (*shelf_ptr)[3] = &comics[1];  // Points to 👾, 🤖, 👽

    // Print Shelf 1
    for (int i = 0; i < 3; i++) {
        printf("%c ", (*shelf_ptr)[i]);  // 👾 🤖 👽
    }

    return 0;
}
```

**ASCII Art:**
```
shelf_ptr (🔦)
    |
    ▼
Shelf 1: [👾, 🤖, 👽]
```

---

#### **2. Pointer to Entire 2D Array (Whole Store)**
```c
// Pointer to the ENTIRE comic store
char (*store_ptr)[3][3] = &comics;

// Access the dragon (Row 2, Column 0)
printf("\nDragon: %c\n", (*store_ptr)[2][0]);  // 🐉
```

**ASCII Art:**
```
store_ptr (🏬)
    |
    ▼
Entire Store:
[
    [🦸, 🦇, 🕷️],
    [👾, 🤖, 👽],
    [🐉, 🦄, 🦕]  ← (*store_ptr)[2][0] = 🐉
]
```

---

### **🚶‍♂️ How It Works**
1. **`char (*shelf_ptr)[3]`**  
   - Points to **one shelf (row)** at a time.  
   - `shelf_ptr + 1` moves to the next shelf.

2. **`char (*store_ptr)[3][3]`**  
   - Points to the **whole store (2D array)**.  
   - Must use `(*store_ptr)[row][col]` to access comics.

---

### **🆚 Pointer Types Comparison**
| Pointer Type | Example | What It Points To | Access Method |
|--------------|---------|-------------------|---------------|
| **Single-element pointer** | `char *ptr` | One comic (e.g., `'🦸'`) | `ptr[0]` |
| **Row pointer** | `char (*shelf_ptr)[3]` | One shelf (row) | `(*shelf_ptr)[1]` |
| **2D array pointer** | `char (*store_ptr)[3][3]` | Entire store | `(*store_ptr)[2][0]` |

---

### **💡 When to Use?**
- **Row pointer**: When processing a 2D array **row-by-row** (e.g., image pixels).  
- **2D array pointer**: Rare—used when passing **entire grids** to functions.  

**Try it!** Modify the example to point to different shelves/comics. 🚀  

#### **Output Examples:**
```
👾 🤖 👽   // Shelf 1
Dragon: 🐉 // Specific comic
```

### Book Notes

The elements of 2-D array can be accessed with the help of pointer notation also. Suppose arr is
a 2-D array, we can access any element arr[i][j] of this array using the pointer expression
`*(*(arr+i)+j)`. 

Let understand this. 

```
int arr[3][4] = { {10,11,12,13}, {20,21,22,23}, {30,31,32,33} };
```

We have been talking about 2-D array in terms of rows and columns, but since memory in computer
is organized lineraly it is not possible to store the 2-D array in rows and columns. The concept of
rows and columns is only theoretical, actually a 2-D array is stored in row major order, i.e. rows
are placed next to each other.

```
arr[0][0]        arr[1][0]          arr[2][0]
    10 11 12 13      20 21 22 23       30 31 32 33
    5000                                        5044
```

Each row can be considered as a 1-D array, so a 2-D array can be considered as a collection of 1-D
arrays that are placed one after another. In other words, we can say that a 2-D array is an array
of arrays. 

We know that name of array is a constant pointer that points to the 0th element of array. In the
case of 2-D arrays, 0th element is a 1-D array, so the name of a 2-D array represents a pointer to
a 1-D array. For example in the above case, arr is pointer to 0th 1-D array and contains address
5000.

Since arr is a `pointer to an array of 4 integers`, according to pointer arithmetic the expression
arr+1 will represent the address 5016 and expression arr+2 will represent adress 5032.

```
arr    --> 10 11 12 13
arr+1  --> 20 21 22 23
arr+2  --> 30 31 32 33
```

In general we can write:

arr + i points to ith element of arr -> points to ith 1-D array

Since arr+i points to ith element of arr, on dereferncing it we'll get ith element of arr which is
of course a 1-D array. Thus the expression *(arr+1) gives us the base address of ith 1-D array.


```
*(arr+0) - arr[0] - Base address of 0th 1-D array - Points to 0th element of 0th 1-D array - 5000
*(arr+1) - arr[1] - Base address of 1st 1-D array - Points to the 1st element of 1st 1-D array - 5016
```

Note that both the expressions (arr+i) and *(arr+1) are pointers, but their base type is different.
The base type of (arr+i) is `an array of 4 ints` while the base type of *(arr+i) or arr[i] is int.

To access an individual element of our 2-D array, we should be able to access any jth element of the
ith 1-D array. 

For example, *(arr+i)+j will represent the address of jth element of ith 1-D array. On dereferencing
this expression we can get the jth element of the ith 1-D array.

```
arr        Points to 0th 1-D array
*arr       Points to 0th element of 0th 1-D array
(arr+i)    Points to ith 1-D array
*(arr+i)   Points to 0th element of ith 1-D array
*(arr+i)+j Points to jth element of the ith 1-D arrat
*(*(arr+i)+j) Represents the value of jth element of ith 1-D array.
```

**Program to print the values and address of a elements of a 2-D array**

```
int arr[3][4] = {
                    {10,11,12,13},
                    {20,21,22,23},
                    {30,31,32,33}
                };

int i, j;
for (i = 0; i < 3; i++)
{
    printf("Address of %dth array = %p %p \n", i ,arr[i], *(arr+i));
    for (j = 0; j < 4; j++)
    {
        printf("%d %d", arr[i][j], *(*(arr+i)+j));
    }
    printf("\n");
}

return 0;

}
```
