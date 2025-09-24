---
title: "Linked Lists Basic"
date: 2025-08-30 00:00:00  +0500
categories: [LinkedList]
tags: [C]
---
List is a collection of similar type of elements. There are two types of ways of mainitaing a list
in memory. The first way is to store the elements of the list in an array, but arrays have some
restrictions and disadvantages. Second way of maintaining a list in memory is through linked list.

### Single Linked List

A single linked list is made up of nodes where each node has two parts, the first one is the info
part that contains the actual data of the list and the second one is the link part that points to
the next node of the list or we can that contains the address of the next node.

In array elements are stored in consecutive memory locations in the same order as they appear in the
list. So array is sequential representation of list while linked list is the linked representation
of list. In a linked list, nodes are not stored contiguously as in arrat, but they are linked through
pointers(links) and we can use these links to move through the list. 

The general form of a node of linked list is - 
```
struct node {
                int value;
                struct node *link;
            };
```

In array we could perform all the operations using the array name and index. In the case of linked list,
we will perform all the operations with the help of the pointer start because it is the only source through
which we can access our linked list. The list will be considered empty if the pointer start conatins
NULL value. So our first job is to declare the pointer start and initialize it to NULL.
This can be done as-

```
struct node *start;
start = NULL;
```

Now we will discuss the following operations on a single linked list.
1. Traversal of a linked list
2. Searching an element
3. Insertion of an element
4. Deletion of an element
5. Creation of a linked list
6. Reversal of a linked list

