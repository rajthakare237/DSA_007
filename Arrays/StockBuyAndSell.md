# Stock Buy and Sell in JavaScript 📈

This repository contains the implementation of the **Stock Buy and Sell** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Stock Buy and Sell Problem?
You are given an array `prices` where `prices[i]` is the price of a given stock on the $i$-th day. You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.

**Core Idea:** You need to find the maximum difference between two numbers in the array, with the strict condition that the smaller number must appear *before* the larger number.

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to check every possible pair of buy and sell days. We use two nested loops: the outer loop selects the day to buy, and the inner loop checks all subsequent days to sell, keeping track of the maximum profit found.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Checks all possible future sell days for every single buy day.
 */
function maxProfitBruteForce(prices) {
    let n = prices.length;
    let maxProfit = 0;
    
    // Outer loop picks the buy day
    for (let i = 0; i < n; i++) {
        // Inner loop picks the sell day (must be in the future)
        for (let j = i + 1; j < n; j++) {
            let currentProfit = prices[j] - prices[i];
            
            // Update maxProfit if the current profit is higher
            if (currentProfit > maxProfit) {
                maxProfit = currentProfit;
            }
        }
    }
    
    return maxProfit;
}

// Example Usage
const prices1 = [7, 1, 5, 3, 6, 4];
console.log("Brute Force Max Profit:", maxProfitBruteForce(prices1)); 
// Output: 5 (Buy at 1, Sell at 6)
```

### Complexity
* **Time Complexity:** $O(N^2)$ where $N$ is the number of days (array length). For large arrays, this will result in a Time Limit Exceeded (TLE) error.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Optimal Approach

Instead of using nested loops, we can solve this in a single pass. As we iterate through the array, we keep track of the **minimum price seen so far**. For every current price, we calculate the profit if we were to sell today (Current Price - Minimum Price). We then keep track of the maximum profit.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Single Pass)
 * Keeps track of the minimum buy price and calculates potential max profit on the go.
 */
function maxProfitOptimal(prices) {
    let maxProfit = 0;
    let minPrice = Infinity; // Start with the highest possible value
    
    for (let i = 0; i < prices.length; i++) {
        // If we find a new lower price, update the minPrice
        if (prices[i] < minPrice) {
            minPrice = prices[i];
        } else {
            // Otherwise, calculate profit if we sold today
            let currentProfit = prices[i] - minPrice;
            
            // Update maxProfit if today's profit is the highest we've seen
            if (currentProfit > maxProfit) {
                maxProfit = currentProfit;
            }
        }
    }
    
    return maxProfit;
}

// Example Usage
const prices2 = [7, 1, 5, 3, 6, 4];
console.log("Optimal Max Profit:", maxProfitOptimal(prices2));
// Output: 5 (Buy at 1, Sell at 6)
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of days. We only iterate through the array exactly once.
* **Space Complexity:** $O(1)$ auxiliary space, as we are only using a couple of variables (`minPrice` and `maxProfit`) regardless of the array size.