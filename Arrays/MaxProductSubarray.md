# Maximum Product Subarray in JavaScript ✖️📈

This repository contains the implementation of the **Maximum Product Subarray** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Maximum Product Subarray Problem?
Given an integer array `nums`, find a contiguous non-empty subarray within the array that has the largest product, and return the product.

**Core Idea:** Unlike the Maximum Subarray *Sum* (Kadane's Algorithm), multiplication introduces a twist: multiplying two negative numbers yields a positive number. Therefore, a highly negative subarray could suddenly become the maximum if multiplied by one more negative number. Additionally, any `0` in the array resets the product to zero.

---

## 1️⃣ Brute Force / Better Approach

The basic way to solve this is to generate all possible subarrays, calculate their products on the fly, and keep track of the maximum product found. We use two nested loops for this.

### JavaScript Code

```javascript
/**
 * Brute Force / Better Approach
 * Calculates the product of all possible subarrays using two nested loops.
 */
function maxProductBruteForce(arr) {
    let n = arr.length;
    let max = -Infinity; // Start with the smallest possible number
    
    for (let i = 0; i < n; i++) {
        let product = 1;
        
        for (let j = i; j < n; j++) {
            product *= arr[j];
            
            // Update max if the current product is greater
            if (product > max) {
                max = product;
            }
        }
    }
    
    return max;
}

// Example Usage
const arr1 = [2, 3, -2, 4];
console.log("Brute Force Max Product:", maxProductBruteForce(arr1)); 
// Output: 6 (Subarray: [2, 3])
```

### Complexity
* **Time Complexity:** $O(N^2)$ where $N$ is the size of the array. We check every combination, which is too slow for very large arrays.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Optimal Approach (Prefix and Suffix Observation)

Instead of checking all subarrays, we can rely on a mathematical observation:
1. If the array has an **even** number of negative values, the max product is the product of the whole array.
2. If the array has an **odd** number of negative values, the max product will either be the product of elements to the *left* of the last negative number, or to the *right* of the first negative number.
3. If the array contains a **zero**, it breaks the array into separate subarrays. We simply reset our product counter to `1` and continue.

By calculating a running product from the front (`prefix`) and a running product from the back (`suffix`) simultaneously, we are guaranteed to find the optimal window.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Prefix and Suffix)
 * Calculates running products from both ends. Resets to 1 when a 0 is encountered.
 */
function maxProductOptimal(arr) {
    let n = arr.length;
    let max = -Infinity;
    let prefix = 1;
    let suffix = 1;
    
    for (let i = 0; i < n; i++) {
        // If we encounter a 0, we reset the running product to 1 
        // because 0 breaks the contiguous product chain.
        if (prefix === 0) prefix = 1;
        if (suffix === 0) suffix = 1;
        
        // Multiply current elements from left (prefix) and right (suffix)
        prefix *= arr[i];
        suffix *= arr[n - 1 - i];
        
        // Update the global maximum
        max = Math.max(max, Math.max(prefix, suffix));
    }
    
    return max;
}

// Example Usage
const arr2 = [2, 3, -2, 4, -1];
console.log("Optimal Max Product:", maxProductOptimal(arr2));
// Output: 48 (Subarray: [2, 3, -2, 4, -1])
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of elements. We traverse the array exactly once (doing both prefix and suffix calculations in the same loop).
* **Space Complexity:** $O(1)$ auxiliary space, as we only use a few variables to track the running products and the maximum value.