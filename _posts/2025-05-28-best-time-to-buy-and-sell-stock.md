---
title: "Best Time to Buy and Sell Stock"
date: 2025-05-28 00:00:00  +0500
categories: [LeetCode]
tags: [Array]
---
**LeetCode Problem** : <https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/>

### Description
You are given an array prices where prices[i] is the price of a given stock on the ith day.

You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

### Example
Input: prices = [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
Here's a complete C solution to generate Pascal's Triangle for a given number of rows:

Here's a C solution to solve the stock buying/selling problem with maximum profit:

### Solution Code
```c
#include <stdio.h>

int maxProfit(int* prices, int pricesSize) {
    if (pricesSize < 2) return 0;

    int minPrice = prices[0];
    int maxProfit = 0;

    for (int i = 1; i < pricesSize; i++) {
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        } else {
            int currentProfit = prices[i] - minPrice;
            if (currentProfit > maxProfit) {
                maxProfit = currentProfit;
            }
        }
    }

    return maxProfit;
}

int main() {
    int prices1[] = {7, 1, 5, 3, 6, 4};
    int size1 = sizeof(prices1)/sizeof(prices1[0]);
    printf("Max profit: %d\n", maxProfit(prices1, size1)); // Output: 5

    int prices2[] = {7, 6, 4, 3, 1};
    int size2 = sizeof(prices2)/sizeof(prices2[0]);
    printf("Max profit: %d\n", maxProfit(prices2, size2)); // Output: 0

    return 0;
}
```

### Explanation

1. **Initialization**:
   - `minPrice` tracks the lowest price seen so far (initialized to first day's price)
   - `maxProfit` tracks the highest profit we can make (initialized to 0)

2. **Single Pass Through Prices**:
   - For each subsequent day's price:
     - If the price is lower than our current `minPrice`, update `minPrice`
     - Otherwise, calculate potential profit if we sold at this price
     - Update `maxProfit` if this profit is better than our current maximum

3. **Edge Cases**:
   - If there's only 1 day or fewer, return 0 (can't make any profit)
   - If prices keep decreasing, `maxProfit` remains 0

4. **Example Walkthrough** (for [7,1,5,3,6,4]):
   - Day 0: minPrice=7, maxProfit=0
   - Day 1: price=1 (<7) → minPrice=1
   - Day 2: price=5 → profit=4 → maxProfit=4
   - Day 3: price=3 → profit=2 → no change
   - Day 4: price=6 → profit=5 → maxProfit=5
   - Day 5: price=4 → profit=3 → no change
   - Final result: 5

### Key Features
- **Time Complexity**: O(n) - Single pass through the array
- **Space Complexity**: O(1) - Uses constant space
- **Efficiency**: Finds solution in one pass without extra memory
- **Correctness**: Handles all edge cases including decreasing prices

This solution efficiently finds the maximum possible profit by keeping track of the minimum price seen so far and calculating potential profits at each step.
