---
title: "Definition vs Declaration"
date: 2025-07-06 00:00:00  +0500
categories: [Tech-Jargons]
tags: [C]
---

### **Defining vs. Declaring a Variable in C (Simple Explanation)**

#### **1. Declaring a Variable**  
- **What it does**: Tells the compiler, *"This variable exists, but I'm not assigning memory yet."*  
- **When to use**: When you need to mention a variable before its actual definition (e.g., in headers).  
- **Example**:  
  ```c
  extern int x;  // Declaration (no memory allocated yet)
  ```
  - `extern` means the variable is defined somewhere else.  

#### **2. Defining a Variable**  
- **What it does**: Tells the compiler, *"Create this variable and allocate memory for it."*  
- **When to use**: When you actually want to use the variable.  
- **Example**:  
  ```c
  int x = 10;  // Definition + initialization (memory allocated)
  ```

---

### **Real-World Analogy**  
- **Declaration** → Like saying, *"I will have a bank account."* (No money yet.)  
- **Definition** → Actually opening the account and depositing money.  

---

### **Complete C Example**  
```c
#include <stdio.h>

// DECLARATION (just tells the compiler "y exists somewhere")
extern int y;  

int main() {
    // DEFINITION (memory allocated)
    int x = 5;  

    // DEFINITION (memory allocated, but no initial value)
    int z;      

    printf("x = %d\n", x);  // Works (x is defined)
    printf("y = %d\n", y);  // Works only if y is DEFINED elsewhere
    printf("z = %d\n", z);  // Works, but z has garbage value

    return 0;
}

// DEFINITION of y (now memory is allocated)
int y = 20;  
```

---

### **Key Differences**  

| **Action**  | **Declaration** | **Definition** |
|-------------|-----------------|----------------|
| **Memory Allocated?** | ❌ No | ✅ Yes         |
| **Can be used?** | ❌ No (unless linked later) | ✅ Yes |
| **Example** | `extern int a;` | `int a = 10;` |

---

### **When to Use Which?**  
1. **Use `declaration`** → When sharing variables across multiple files (in headers).  
2. **Use `definition`** → When actually creating and using a variable in your code.  

---

### **Common Mistakes**  
❌ **Declaring without defining** → Leads to linker errors.  
```c
extern int missing_var;  
printf("%d", missing_var);  // ERROR if missing_var isn't defined anywhere
```

✅ **Fix**: Ensure every `extern` declaration has a corresponding definition somewhere.  

---

### **Summary**  
- **Declaration** → *"This variable exists, but memory isn’t allocated yet."*  
- **Definition** → *"Create this variable and assign memory."*  

Now you can confidently use both in your C programs! 🚀
