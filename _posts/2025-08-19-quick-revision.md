---
title: "Pointer Memory Model"
date: 2025-07-05 00:00:00  +0500
categories: [Quick Revision]
tags: [C]
---


### Difference Types of Pointers

1. Data Pointer : where we have data type mention ex: int *ptr etc.
2. Function Pointer : that point to piece of code when we dereference it is same as calling a function
3. Void Pointer : it is just a pointer which can point to data or function and dereference is not allowed.


#### What Is a Function Pointer?

A function pointer is a variable that stores the address of a function.

Instead of calling a function directly by name, you can invoke it through the pointer.

Imagine functions are like tools in a toolbox. A function pointer is like a label you can move between tools.
It doesn't do the work, but it points to which tool to use


#### What Is a Callback Function?
A callback function is a function passed as an argument to another function.

It allows the receiving function to "call back" the passed-in function at the appropriate time.

In C, this is done using function pointers.

```
#include <stdio.h>

// This is your "phone number" — the callback
void pizzaReady() {
    printf("Your pizza is ready! 🍕\n");
}

// This is the pizza place — it takes your number and calls you later
void orderPizza(void (*callback)()) {
    printf("Pizza is being prepared...\n");
    // After some time...
    callback();  // Calls you back
}

int main() {
    orderPizza(pizzaReady);  // You give them your number
    return 0;
}
```
