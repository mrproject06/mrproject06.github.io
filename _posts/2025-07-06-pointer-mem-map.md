---
title: "Pointer Memory Model"
date: 2025-07-05 00:00:00  +0500
categories: [Mastering C Pointers]
tags: [C]
---

Pointer is a variable just like any other variable which stores the address of another variable.

```
int a;
int *ptr = &a; /* ptr is a pointer that stores the address of variable `a` */
```

```
char c = 'A';
char *c_ptr = &c; /* c_ptr is a pointer of type char which stores the adderss of variable c */
```

`& is a operand which when appended to any variable denotes the address of variable`

`* is a dereference operator which gets the element stored at particular address pointed by pointer
variable`

`Multiplication & Division on pointer variable is not possible`

```
*&i = i
*&*&*&i = 20 / *
```

### Double Pointer

```
int *pi = &i;
int **ppi = &pi; /* A pointer to pointer */
int ***pppi = &ppi; 
```

### Array of Pointers

```
#include <stdio.h>
#include <stdlib.h>

int main(void) {
    char *p[10];

    for (int i = 0; i<10; i++) {
        p[i] = (char *)malloc(1); /* Allocating dynamic memory of 1 byte */
        *p[i] = 10 - i;
        printf("p[%d]: %p\n", i, p[i]);
    }

    for (int i = 0; i<10; i++) {
        printf("*p[%d]: %d\n", i, *p[i]);
    }
    
    printf("sizeof(p): %d\n", sizeof(p));

    for (int i = 0; i<10; i++) {
        free(p[i]);
    }
    return 0;
}
```


### Difference Types of Pointers

1. Data Pointer : where we have data type mention ex: int *ptr etc.
2. Function Pointer : that point to piece of code when we dereference it is same as calling a function
3. Void Pointer : it is just a pointer which can point to data or function and dereference is not allowed.

### Pointer pointing to a data type

```
char c = 'A';

char *p = &c;

*p : dereference operation knows that it has to fetch only 1 byte because of data type specified.

int i = 10;
int *ptr = &i;

*ptr : It has to fetch 4 bytes from the memory address stored in ptr pointer variable
```

### Pointer to a struct

```
struct my_struct {
    long long int a; // 8 bytes
    float b;         // 4 bytes
    int c;           // 4 bytes
};

int main () {

struct my_struct var = {
    .a = 1024,
    .b = 2.5,
    .c = -1
};

struct my_struct *ptr = &var;

/* Dereferencing the struct members */
(*ptr).a = (*ptr).a + 1;
(*ptr).b = (*ptr).b + 1;
(*ptr).c = (*ptr).c + 1;

printf("var.a :%lld, var.b: %f, var.c:%d\n", var.a, var.b, var.c);
printf("(*ptr).a: %lld, (*ptr).b: %f, (*ptr).c: %d\n\n", (*ptr).a, (*ptr).b, (*ptr).c);

/* Shorthand */
ptr->a = ptr->a + 1;
ptr->b = ptr->b + 1;
ptr->c = ptr->c + 1;

printf("ptr->a: %lld, ptr->b: %f, ptr->c:%d\n", ptr->a, ptr->b, ptr->c);

return 0;
}
```

### Function Pointer or callbacks: Pointer to memory region where code is present

```

void function_1() {
    printf("function_1()\n");
}

void function_2() {
    printf("function_2()\n");
}

int main() {
    void (*func_ptr)();

    func_ptr = function_1;
    func_ptr();

    func_ptr = function_2;
    func_ptr();

    return 0;
}

```
### Function pointer with input parameter

```
void function_1(int a) {
    printf("function_1(): a = %d\n");
}

void function_2(int b) {
    printf("function_2(): b = %d\n");
}


int main() {
    void (*func_ptr)(int);
    /*
    typedef void (*func_ptr)(int); // This is another way

    func_ptr pf;
    */
    func_ptr = function_1;
    func_ptr(10);

    func_ptr = &function_2; // & is optional
    func_ptr(20);

    return 0;
}

```

### Function Pointer Array

```
#include "math_library.h"

int main() {
    execute_operation(ADD, 100, 5);
    execute_operation(SUB, 100, 5);
    execute_operation(MUL, 100, 5);
    execute_operation(DIV, 100, 5);
    return 0;
}

Header File

#define ADD 0
#define SUB 1
#define MUL 2
#define DIV 3

void execute_operation(int operation, int a, int b);

Implementatio of lib

#include <stdio.h>
#include "math_library.h"

static void add (int x, int y) {
    printf("Add: %d\n", x+y);
}

static void sub (int x, int y) {
    printf("Sub: %d\n", x-y);
}

static void mul (int x, int y) {
    printf("Mul: %d\n", x*y);
}

static void div (int x, int y) {
    printf("Div: %d\n", x/y);
}

typedef void (*func_t)(int a, int b);

func_t func_lib[4] = {add,sub,mul,div};

void execute_operation(int operation, int a, int b) {
    func_lib[operation](a,b)
}

```

```
Here’s a detailed explanation of lines 23 to 34 in math_library.c:

typedef void (*operation_func)(int, int);

This line defines a new type, operation_func, which is a pointer to a function that takes two int arguments and returns void. This allows you to create arrays or variables that can point to any function matching this signature (like add, subtract, etc.).
operation_func operations[] = { add, subtract, multiply, divide };

This declares an array called operations where each element is a function pointer of type operation_func.
The array is initialized with the addresses of four functions: add, subtract, multiply, and divide. Each of these functions matches the signature defined above.
void execute_operation(int a, int b, int operation) { operations[operation](a, b); }

This function, execute_operation, takes three arguments: two integers (a, b) and an integer operation.
Inside the function, it uses the operation integer as an index to select the appropriate function from the operations array, then calls that function with a and b as arguments.
For example, if operation is 0, it calls add(a, b); if 1, it calls subtract(a, b), and so on.
This pattern allows dynamic selection and execution of mathematical operations using function pointers, making the code modular and extensible.
```

### Dynamic Memory Allocation

```

void *malloc(size_t size);
void free(void *ptr);

#include <stdio.h>
#include <stdlib.h>

int main() {
    int *p = (int *)malloc(4);

    printf("before: %d\n", *p);
    *p = 4;
    printf("after: %d\n", *p);

    free(p);
    return 0;
}
```

What happens when malloc fails ?

```
size_t unsigned int 

#define REQUEST 1*1024*1024*1024 
#include <stdio.h>
#include <stdlib.h>

int main() {

    int *p;

    for (int i = 0; i <100000; i++) {
        p = (int *)malloc(REQUEST);
        printf("p: %p\n", p);
        if ( p == NULL)
            return -1;
    }

    //free(p);
    return 0;
}
```

### Dangling Pointer

```
malloc()
free()

int *p = (int*)malloc(4);
.
...
....
free(p);
*p = 10; // p is still holding value from previous allocation 
*q // using q now but suppose eariler q was 45 and pointing to same memory

//using a memory location after free, hence P = NULL is advisable

```
