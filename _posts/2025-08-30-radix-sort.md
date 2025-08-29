---
title: "Radix Sort"
date: 2025-08-30 00:00:00  +0500
categories: [Sorting]
tags: [C]
---
Of course! Let me explain the passes in Radix Sort more clearly with a simpler example and visual steps.

## **🧠 Layman's Explanation: The Multi-Layered Sorting**

**Think of it like sorting a deck of cards multiple times:**
1. **First pass:** Sort all cards by their **suit** (♠️ ♥️ ♦️ ♣️)
2. **Second pass:** Within each suit, sort by **number** (A, 2, 3..., K)
3. **Third pass:** Within each number, sort by **color** (red/black)

**Radix Sort does the same with digits!**

---

## **📝 Simple Example: Sorting [170, 45, 75, 90]**

### **Step 0: Find Maximum Number**
```
Max number: 170 (3 digits)
So we need 3 passes (ones, tens, hundreds)
```

---

## **🔢 Pass 1: Sort by ONES Digit (Rightmost Digit)**

**Look only at the last digit of each number:**

```
170 → look at 0
45  → look at 5  
75  → look at 5
90  → look at 0
```

**Group by ones digit:**
```
Group 0 (ends with 0): 170, 90
Group 5 (ends with 5): 45, 75
```

**Combine groups in order:** `[170, 90, 45, 75]`

**Visualization:**
```
Before: [170, 45, 75, 90]
Sort by ones digit: 
  170 (0), 90 (0) → Bucket 0
  45 (5), 75 (5) → Bucket 5
After: [170, 90, 45, 75]
```

---

## **🔢 Pass 2: Sort by TENS Digit (Middle Digit)**

**Now look at the second digit of each number:**

```
170 → look at 7 (tens digit)
90  → look at 9 (tens digit)  
45  → look at 4 (tens digit)
75  → look at 7 (tens digit)
```

**Group by tens digit:**
```
Group 4 (tens digit 4): 45
Group 7 (tens digit 7): 170, 75
Group 9 (tens digit 9): 90
```

**Combine groups in order:** `[45, 170, 75, 90]`

**Visualization:**
```
Before: [170, 90, 45, 75]
Sort by tens digit:
  45 (4) → Bucket 4
  170 (7), 75 (7) → Bucket 7
  90 (9) → Bucket 9
After: [45, 170, 75, 90]
```

---

## **🔢 Pass 3: Sort by HUNDREDS Digit (Leftmost Digit)**

**Now look at the first digit of each number:**

```
45   → look at 0 (045 - no hundreds digit)
170  → look at 1
75   → look at 0 (075 - no hundreds digit)  
90   → look at 0 (090 - no hundreds digit)
```

**Group by hundreds digit:**
```
Group 0 (hundreds digit 0): 45, 75, 90
Group 1 (hundreds digit 1): 170
```

**Combine groups in order:** `[45, 75, 90, 170]`

**Visualization:**
```
Before: [45, 170, 75, 90]
Sort by hundreds digit:
  45 (0), 75 (0), 90 (0) → Bucket 0
  170 (1) → Bucket 1
After: [45, 75, 90, 170]
```

**Final Sorted Array:** `[45, 75, 90, 170]`

---

## **🎯 Key Insight: Why Multiple Passes Work**

### **Like Sorting by Multiple Criteria:**
1. **First:** Sort by last name
2. **Then:** Sort by first name (within same last names)
3. **Finally:** Sort by middle name (within same first names)

### **The Magic:**
- Each pass **preserves the order** from previous passes
- It's **stable sort** - equal digits keep their relative order
- We start from **least significant** to **most significant** digit

---

## **📊 Another Visualization**

**Original:** `[802, 2, 24]`

**Pass 1 (ones digit):**
```
802 → 2
2   → 2  
24  → 4

Group: 2's: [802, 2], 4's: [24]
Result: [802, 2, 24] (order preserved for equal 2's)
```

**Pass 2 (tens digit):**
```
802 → 0 (80**2** → tens digit is 0? Wait, let's correct)
802 → 802: hundreds=8, tens=0, ones=2
2   → 0 (02 → tens digit 0)
24  → 2

Group: 0's: [802, 2], 2's: [24]
Result: [802, 2, 24]
```

**Pass 3 (hundreds digit):**
```
802 → 8
2   → 0  
24  → 0

Group: 0's: [2, 24], 8's: [802]
Result: [2, 24, 802]
```

---

## **🔍 How Radix Sort Preserves Order (Stability)**

**This is the magic sauce!**

When two numbers have the **same current digit**, they stay in the **same relative order** as the previous pass.

**Example:** `[802, 2]` both have ones digit = 2
- After pass 1: `[802, 2]` (not `[2, 802]`)
- This allows later passes to correctly sort them

---

## **🎓 Simple Analogies**

### **1. Library Books:**
- **Pass 1:** Sort by author's last name
- **Pass 2:** Sort by author's first name (within same last names)
- **Pass 3:** Sort by book title (within same authors)

### **2. School Records:**
- **Pass 1:** Sort by grade level
- **Pass 2:** Sort by class (within same grade)
- **Pass 3:** Sort by roll number (within same class)

### **3. Coffee Shop:**
- **Pass 1:** Sort by drink type (coffee, tea, smoothie)
- **Pass 2:** Sort by size (within each type)
- **Pass 3:** Sort by order time (within each size)

## **⚡ Why This Order Matters**

We sort from **least significant** to **most significant** digit because:
1. The **earlier passes** create rough grouping
2. The **later passes** refine the sorting
3. It **preserves stability** - crucial for correct sorting

**Think of it like:** First sorting by city, then by street, then by house number - this gives you perfect geographical order!

Radix Sort is like doing multiple simple sorts that build upon each other to create a perfectly sorted result!


```
#include <stdio.h>
#include <stdlib.h>

// Function to get maximum value in array
int getMax(int arr[], int n) {
    int max = arr[0];
    for (int i = 1; i < n; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }
    return max;
}

// Counting sort for specific digit
void countingSort(int arr[], int n, int exp) {
    int output[n];          // Output array
    int count[10] = {0};    // Count array for digits 0-9

    // Store count of occurrences
    for (int i = 0; i < n; i++) {
        int digit = (arr[i] / exp) % 10;
        count[digit]++;
    }

    // Change count[i] to actual position
    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    // Build output array
    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    // Copy output to original array
    for (int i = 0; i < n; i++) {
        arr[i] = output[i];
    }
}

// Radix Sort
void radixSort(int arr[], int n) {
    int max = getMax(arr, n);

    // Do counting sort for every digit
    for (int exp = 1; max / exp > 0; exp *= 10) {
        countingSort(arr, n, exp);
    }
}

int main() {
    int arr[] = {170, 45, 75, 90, 802, 24, 2, 66};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("Original: ");
    for (int i = 0; i < n; i++) printf("%d ", arr[i]);

    radixSort(arr, n);

    printf("\nSorted:   ");
    for (int i = 0; i < n; i++) printf("%d ", arr[i]);

    return 0;
}
```


Let me explain this Radix Sort code step-by-step with the example array `[170, 45, 75, 90, 802, 24, 2, 66]`.

## **🧠 Radix Sort Overview**
Radix Sort works by processing digits from least significant to most significant, using Counting Sort as a subroutine for each digit position.

---

## **📝 Step-by-Step Execution**

### **Initial Array:** `[170, 45, 75, 90, 802, 24, 2, 66]`

### **Step 1: Find Maximum Number**
```c
int max = getMax(arr, n);  // Returns 802
```
**Maximum number:** 802 (3 digits)  
**So we need 3 passes:** ones, tens, and hundreds places

---

## **Pass 1: Sort by Ones Digit (exp = 1)**

### **Extract Ones Digits:**
```
170 → (170/1) % 10 = 0
45  → (45/1) % 10 = 5
75  → (75/1) % 10 = 5
90  → (90/1) % 10 = 0
802 → (802/1) % 10 = 2
24  → (24/1) % 10 = 4
2   → (2/1) % 10 = 2
66  → (66/1) % 10 = 6
```

### **Count Occurrences:**
```
Digit 0: 2 times (170, 90)
Digit 2: 2 times (802, 2)
Digit 4: 1 time (24)
Digit 5: 2 times (45, 75)
Digit 6: 1 time (66)
```

### **After Counting Sort by Ones Digit:**
**Array becomes:** `[170, 90, 802, 2, 24, 45, 75, 66]`

**Visual Grouping:**
- Ends with 0: 170, 90
- Ends with 2: 802, 2
- Ends with 4: 24
- Ends with 5: 45, 75
- Ends with 6: 66

---

## **Pass 2: Sort by Tens Digit (exp = 10)**

### **Extract Tens Digits:**
```
170 → (170/10) % 10 = 7
90  → (90/10) % 10 = 9
802 → (802/10) % 10 = 0
2   → (2/10) % 10 = 0
24  → (24/10) % 10 = 2
45  → (45/10) % 10 = 4
75  → (75/10) % 10 = 7
66  → (66/10) % 10 = 6
```

### **Count Occurrences:**
```
Digit 0: 2 times (802, 2)
Digit 2: 1 time (24)
Digit 4: 1 time (45)
Digit 6: 1 time (66)
Digit 7: 2 times (170, 75)
Digit 9: 1 time (90)
```

### **After Counting Sort by Tens Digit:**
**Array becomes:** `[802, 2, 24, 45, 66, 170, 75, 90]`

**Visual Grouping:**
- Tens digit 0: 802, 2
- Tens digit 2: 24
- Tens digit 4: 45
- Tens digit 6: 66
- Tens digit 7: 170, 75
- Tens digit 9: 90

---

## **Pass 3: Sort by Hundreds Digit (exp = 100)**

### **Extract Hundreds Digits:**
```
802 → (802/100) % 10 = 8
2   → (2/100) % 10 = 0
24  → (24/100) % 10 = 0
45  → (45/100) % 10 = 0
66  → (66/100) % 10 = 0
170 → (170/100) % 10 = 1
75  → (75/100) % 10 = 0
90  → (90/100) % 10 = 0
```

### **Count Occurrences:**
```
Digit 0: 6 times (2, 24, 45, 66, 75, 90)
Digit 1: 1 time (170)
Digit 8: 1 time (802)
```

### **After Counting Sort by Hundreds Digit:**
**Array becomes:** `[2, 24, 45, 66, 75, 90, 170, 802]`

**Visual Grouping:**
- Hundreds digit 0: 2, 24, 45, 66, 75, 90
- Hundreds digit 1: 170
- Hundreds digit 8: 802

---

## **✅ Final Sorted Array:** `[2, 24, 45, 66, 75, 90, 170, 802]`

---

## **🔍 How countingSort() Works for One Digit**

Let's trace `countingSort(arr, n, 10)` for tens digit:

### **Step 1: Count Occurrences**
```
Count array: [0,0,0,0,0,0,0,0,0,0]
170 → digit 7 → count[7]++
90  → digit 9 → count[9]++  
802 → digit 0 → count[0]++
2   → digit 0 → count[0]++
24  → digit 2 → count[2]++
45  → digit 4 → count[4]++
75  → digit 7 → count[7]++
66  → digit 6 → count[6]++

Final count: [2,0,1,0,1,0,1,2,0,1]
```

### **Step 2: Calculate Cumulative Counts**
```
count[0] = 2
count[1] = 2 + 0 = 2
count[2] = 2 + 1 = 3
count[3] = 3 + 0 = 3
count[4] = 3 + 1 = 4
count[5] = 4 + 0 = 4
count[6] = 4 + 1 = 5
count[7] = 5 + 2 = 7
count[8] = 7 + 0 = 7
count[9] = 7 + 1 = 8
```

### **Step 3: Build Output Array (Right to Left)**
Process from end to start for stability:
```
66  → digit 6 → position 5-1=4 → output[4]=66
75  → digit 7 → position 7-1=6 → output[6]=75
45  → digit 4 → position 4-1=3 → output[3]=45
24  → digit 2 → position 3-1=2 → output[2]=24
2   → digit 0 → position 2-1=1 → output[1]=2
802 → digit 0 → position 2-1=1? Wait, count[0] was 2
Actually, let's do it properly:

Start with cumulative counts: [2,2,3,3,4,4,5,7,7,8]

Process right to left:
66  → digit 6 → count[6]=5 → position 4 → output[4]=66, count[6]=4
75  → digit 7 → count[7]=7 → position 6 → output[6]=75, count[7]=6
45  → digit 4 → count[4]=4 → position 3 → output[3]=45, count[4]=3
24  → digit 2 → count[2]=3 → position 2 → output[2]=24, count[2]=2
2   → digit 0 → count[0]=2 → position 1 → output[1]=2, count[0]=1
802 → digit 0 → count[0]=1 → position 0 → output[0]=802, count[0]=0
90  → digit 9 → count[9]=8 → position 7 → output[7]=90, count[9]=7
170 → digit 7 → count[7]=6 → position 5 → output[5]=170, count[7]=5

Output: [802, 2, 24, 45, 66, 170, 75, 90]
```

---

## **🎯 Why Radix Sort Works**

1. **Stability**: Counting Sort preserves order of equal digits
2. **LSD First**: Process least significant digit first
3. **Multi-pass**: Each pass refines the sorting
4. **Linear Time**: O(d×n) where d is number of digits

## **⚡ Time Complexity**
- **Best/Average/Worst**: O(d×n)
- d = number of digits in maximum number
- n = number of elements

## **📊 Key Features**
- **Non-comparative**: Doesn't compare elements directly
- **Stable**: Preserves order of equal elements
- **Integer sorting**: Works best with integers
- **Fixed-length**: Assumes all numbers have same number of digits (padded with 0's)

This Radix Sort implementation efficiently sorts integers by processing one digit at a time from least to most significant!
