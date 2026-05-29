# Kadane's Algorithm (Maximum Subarray Sum) in JavaScript 🚀

This repository contains the implementation of the **Maximum Subarray Sum** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Maximum Subarray Sum Problem?
Given an integer array `arr`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

**Core Idea:** A subarray is a contiguous part of an array. We need to find the specific sequence of adjacent elements that yields the maximum possible total when added together.

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to generate all possible subarrays, calculate the sum of each, and keep track of the maximum sum found. We use three nested loops: two to set the start and end points of the subarray, and a third to calculate the sum between those points.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Generates all subarrays and calculates their sum using a third loop.
 */
function maxSubarraySumBruteForce(arr) {
    let n = arr.length;
    let max = -Infinity; // Start with the smallest possible number
    
    // Outer loop for the starting point
    for (let i = 0; i < n; i++) {
        // Inner loop for the ending point
        for (let j = i; j < n; j++) {
            let sum = 0;
            
            // Third loop to calculate the sum of the current subarray [i...j]
            for (let k = i; k <= j; k++) {
                sum += arr[k];
            }
            
            // Update max if the current sum is greater
            if (sum > max) {
                max = sum;
            }
        }
    }
    
    return max;
}

// Example Usage
const arr1 = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
console.log("Brute Force Max Sum:", maxSubarraySumBruteForce(arr1)); 
// Output: 6 (Subarray: [4, -1, 2, 1])
```

### Complexity
* **Time Complexity:** $O(N^3)$ where $N$ is the length of the array. This is highly inefficient and will cause a Time Limit Exceeded (TLE) error for even moderately sized arrays.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Better Approach

We can eliminate the third loop by calculating the sum on the fly. Instead of recalculating the sum from scratch for every new endpoint `j`, we can simply add the current element `arr[j]` to the previous sum.

### JavaScript Code

```javascript
/**
 * Better Approach
 * Removes the third loop by keeping a running sum for subarrays starting at 'i'.
 */
function maxSubarraySumBetter(arr) {
    let n = arr.length;
    let max = -Infinity;
    
    // Outer loop for the starting point
    for (let i = 0; i < n; i++) {
        let sum = 0;
        
        // Inner loop adds elements to the sum one by one
        for (let j = i; j < n; j++) {
            sum += arr[j];
            
            if (sum > max) {
                max = sum;
            }
        }
    }
    
    return max;
}

// Example Usage
const arr2 = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
console.log("Better Max Sum:", maxSubarraySumBetter(arr2));
// Output: 6 
```

### Complexity
* **Time Complexity:** $O(N^2)$ because we are using two nested loops. This is better, but still too slow for massive arrays (e.g., $10^5$ elements).
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 3️⃣ Optimal Approach (Kadane's Algorithm)

**Kadane's Algorithm** allows us to solve this in a single pass. 

**The Logic:** We maintain a running `sum`. If the `sum` ever drops below zero, it means the current subarray is a net negative and will only drag down any future elements we add to it. Therefore, if `sum < 0`, we abandon the current subarray by resetting `sum` to `0` and start a fresh subarray from the next element.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Kadane's Algorithm)
 * Maintains a running sum and resets it to 0 if it becomes negative.
 */
function maxSubarraySumOptimal(arr) {
    let max = -Infinity;
    let sum = 0;
    
    for (let i = 0; i < arr.length; i++) {
        // Step 1: Add current element to the running sum
        sum += arr[i];
        
        // Step 2: Update max if the current running sum is higher
        if (sum > max) {
            max = sum;
        }
        
        // Step 3: If the sum falls below zero, reset it. 
        // A negative sum will only decrease the value of future subarrays.
        if (sum < 0) {
            sum = 0;
        }
    }
    
    // Note: If the problem asks for an empty subarray when all elements are negative, 
    // you would add a check here: if (max < 0) return 0;
    
    return max;
}

// Example Usage
const arr3 = [-2, 1, -3, 4, -1, 2, 1, -5, 4];
console.log("Kadane's Algorithm Max Sum:", maxSubarraySumOptimal(arr3));
// Output: 6 
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of elements. We iterate through the array exactly once.
* **Space Complexity:** $O(1)$ auxiliary space, as we are only tracking two variables (`sum` and `max`).