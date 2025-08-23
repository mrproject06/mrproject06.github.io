---
title: "Sorting Cheat Sheet"
date: 2025-08-23 00:00:00  +0500
categories: [Quick Revision]
tags: [C]
---

Absolutely! Here's a simple cheat sheet with analogies and comparisons to help you easily remember and distinguish between these three sorting algorithms.

## **🎯 Quick Comparison Table**

| Aspect | **Selection Sort** | **Bubble Sort** | **Insertion Sort** |
| :--- | :--- | :--- | :--- |
| **Basic Idea** | "Find the smallest and put it first" | "Bubble up the largest to the end" | "Insert each card in its correct position" |
| **How it works** | Selects smallest element and swaps | Compares adjacent elements and swaps | Builds sorted array one element at a time |
| **Best for** | Small arrays | Educational purposes | Small or nearly-sorted arrays |
| **Time Complexity** | O(n²) always | O(n²) worst, O(n) best | O(n²) worst, O(n) best |
| **Memory** | O(1) | O(1) | O(1) |
| **Stable?** | No | Yes | Yes |

---

## **🧠 Memory Shortcuts & Analogies**

### **1. Selection Sort: "The Picky Shopper"**
- **Analogy**: You're shopping for fruits. You look through the entire basket, find the smallest apple, and put it in your bag first. Then repeat for the next smallest.
- **Mantra**: *"Find minimum, swap with front"*
- **Visual**: `[✓, ✓, ✓, | 7, 3, 9]` ← Keeps adding checked marks

### **2. Bubble Sort: "The Bubbly Soda"**
- **Analogy**: Bubbles rising in soda - the largest elements "bubble up" to the top/end with each pass.
- **Mantra**: *"Compare neighbors, swap if wrong"*
- **Visual**: `[3, 5, 2, 8] → [3, 2, 5, 8] → [2, 3, 5, 8]` ← Bubbles moving

### **3. Insertion Sort: "The Card Player"**
- **Analogy**: Sorting playing cards in your hand - you take one card at a time and insert it where it belongs.
- **Mantra**: *"Take one, insert in sorted part"*
- **Visual**: `[5, 7 | 2, 9] → [2, 5, 7 | 9]` ← Building sorted section

---

## **⚡ Quick Identification Tips**

### **When you see this pattern, it's SELECTION SORT:**
```c
for (i = 0; i < n-1; i++) {
    min = i;
    for (j = i+1; j < n; j++) {    // ← Inner loop starts from i+1
        if (arr[j] < arr[min])     // ← Finding minimum
            min = j;
    }
    swap(arr[i], arr[min]);        // ← Single swap per iteration
}
```

### **When you see this pattern, it's BUBBLE SORT:**
```c
for (i = 0; i < n-1; i++) {
    for (j = 0; j < n-i-1; j++) {  // ← Inner loop goes from start to n-i-1
        if (arr[j] > arr[j+1]) {   // ← Comparing adjacent elements
            swap(arr[j], arr[j+1]); // ← Swapping neighbors
        }
    }
}
```

### **When you see this pattern, it's INSERTION SORT:**
```c
for (i = 1; i < n; i++) {          // ← Starts from index 1
    key = arr[i];
    j = i-1;
    while (j >= 0 && arr[j] > key) { // ← Shifting elements to make space
        arr[j+1] = arr[j];          // ← Moving elements right
        j--;
    }
    arr[j+1] = key;                 // ← Inserting in correct position
}
```

---

## **🎮 Real-life Analogies**

### **Selection Sort**:
- **Like** picking the best students one by one for a team
- **Method**: Scan entire class, pick best, repeat

### **Bubble Sort**:
- **Like** arranging people by height in a line
- **Method**: Compare adjacent people, swap if wrong order

### **Insertion Sort**:
- **Like** sorting books on a shelf one by one
- **Method**: Take each book, find where it belongs, insert it

---

## **📊 Performance Cheat Sheet**

| Scenario | Best Choice | Why |
| :--- | :--- | :--- |
| **Small array** | Insertion Sort | Fast for small n, simple |
| **Nearly sorted** | Insertion Sort | Almost O(n) time |
| **Memory limited** | Any (all are O(1)) | All use constant space |
| **Learning purpose** | Bubble Sort | Easiest to understand |
| **Minimum writes** | Selection Sort | Only n swaps total |

---

## **🔑 Key One-Liner Summaries**

- **Selection**: "Find the smallest, swap it to the front" 🔍
- **Bubble**: "Repeatedly swap adjacent if wrong order" 💭
- **Insertion**: "Build sorted array by inserting one by one" 🃏

**Remember**: All three are O(n²) in worst case, but Insertion Sort is usually the most practical of the three for small datasets!

Which one do you find most confusing? I can provide more specific examples!
