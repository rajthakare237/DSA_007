# Nth Fibonacci Number in JavaScript 🌀

This repository contains the implementation of the **Nth Fibonacci Number** problem in JavaScript, following the Dynamic Programming teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
The Fibonacci numbers, commonly denoted $F(n)$, form a sequence where each number is the sum of the two preceding ones, starting from `0` and `1`.
* $F(0) = 0, F(1) = 1$
* $F(n) = F(n - 1) + F(n - 2)$, for $n > 1$.

Given `n`, calculate $F(n)$.

---

## 1️⃣ Brute Force Approach (Standard Recursion)

The most intuitive way to write this is to directly translate the mathematical recurrence relation into a recursive function. However, this results in calculating the same overlapping subproblems repeatedly.

### JavaScript Code

```javascript
/**
 * Brute Force Approach (Recursion)
 * Directly translates the mathematical formula into recursive calls.
 */
function fibRecursion(n) {
    // Base cases: F(0) = 0, F(1) = 1
    if (n <= 1) {
        return n;
    }
    
    // Recursive calls for the two preceding numbers
    return fibRecursion(n - 1) + fibRecursion(n - 2);
}

// Example Usage
const n1 = 6;
console.log("Brute Force (Recursion):", fibRecursion(n1)); 
// Output: 8
```

### Complexity
* **Time Complexity:** $O(2^N)$ (exponential). The function tree branches into two at every step, causing a massive recalculation of the same values. For large numbers (e.g., $N=50$), this will freeze your program.
* **Space Complexity:** $O(N)$ auxiliary space, strictly due to the recursive call stack reaching a maximum depth of $N$.

---

## 2️⃣ Better Approach (Top-Down DP / Memoization)

To fix the overlapping subproblems, we can use **Memoization**. We create an array (or map) to store the results of subproblems as we compute them. Before making a recursive call, we check if the answer is already in our "memo". If it is, we return it instantly in $O(1)$ time.

### JavaScript Code

```javascript
/**
 * Better Approach (Memoization)
 * Uses an array to cache previously computed Fibonacci numbers.
 */
function fibMemoization(n, memo = []) {
    // Base cases
    if (n <= 1) {
        return n;
    }
    
    // If the value has already been computed, return it from the cache
    if (memo[n] !== undefined) {
        return memo[n];
    }
    
    // Compute the value, store it in the cache, and then return it
    memo[n] = fibMemoization(n - 1, memo) + fibMemoization(n - 2, memo);
    
    return memo[n];
}

// Example Usage
const n2 = 6;
console.log("Better (Memoization):", fibMemoization(n2));
// Output: 8
```

### Complexity
* **Time Complexity:** $O(N)$. Every number from $0$ to $N$ is calculated exactly once. Subsequent calls fetch the value in $O(1)$ time.
* **Space Complexity:** $O(N)$ for the recursion stack + $O(N)$ for the `memo` array. Total auxiliary space is still $O(N)$.

---

## 3️⃣ Optimal Approach (Bottom-Up Iteration / Space Optimized)

We can completely eliminate the recursive call stack and the memoization array. 
If we observe the sequence, to calculate the current state `i`, we only ever need to know the **previous two states** (`i-1` and `i-2`). We don't need to keep the entire history in memory. 

### JavaScript Code

```javascript
/**
 * Optimal Approach (Space Optimized Iteration)
 * Uses two variables to keep track of the previous two sequence numbers.
 */
function fibOptimal(n) {
    if (n <= 1) return n;
    
    // Keep track of the only two values we actually need
    let prev2 = 0; // Represents F(n-2)
    let prev1 = 1; // Represents F(n-1)
    
    for (let i = 2; i <= n; i++) {
        let current = prev1 + prev2;
        
        // Shift our variables forward for the next iteration
        prev2 = prev1;
        prev1 = current;
    }
    
    // prev1 will hold the Nth Fibonacci number at the end of the loop
    return prev1; 
}

// Example Usage
const n3 = 6;
console.log("Optimal (Space Optimized):", fibOptimal(n3));
// Output: 8
```

### Complexity
* **Time Complexity:** $O(N)$. We use a simple `for` loop that runs $N-1$ times.
* **Space Complexity:** $O(1)$ auxiliary space. We only use three simple integer variables (`prev2`, `prev1`, `current`), drastically improving memory efficiency compared to the recursive approaches.