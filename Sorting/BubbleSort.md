# Bubble Sort Algorithm in JavaScript 🫧

This repository contains the implementation of the **Bubble Sort** algorithm in JavaScript. The progression follows the standard teaching pattern from [takeuforward.org](https://takeuforward.org), moving from the brute-force method to the optimized version.

---

## 📖 What is Bubble Sort?
Bubble Sort is a simple sorting algorithm that repeatedly steps through the input list element by element, comparing the current element with the one after it, swapping their values if needed. 

**Core Idea:** In every step (or pass), the maximum element is pushed to the last index of the unsorted portion of the array.

---

## 1️⃣ Standard / Brute Force Approach

The standard approach uses two nested loops. The outer loop shrinks the unsorted portion of the array from the end, while the inner loop compares adjacent elements and pushes the largest to the back.

### JavaScript Code

```javascript
function bubbleSortBruteForce(arr) {
    let n = arr.length;
    
    for (let i = n - 1; i >= 1; i--) {
        for (let j = 0; j <= i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                let temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    return arr;
}

// Example Usage
const arr1 = [13, 46, 24, 52, 20, 9];
console.log("Brute Force Sorted:", bubbleSortBruteForce(arr1));
```

### Complexity
* **Time Complexity:** $O(N^2)$ for all cases (Worst, Average, and Best). The loops will run entirely even if the array is already sorted.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Optimized Approach

**The Problem:** Even if the array is already completely sorted, the brute-force algorithm will continue to run all redundant comparisons, resulting in an $O(N^2)$ time complexity.

**The Solution:** We introduce a `didSwap` boolean flag. If the inner loop goes through an entire pass without performing a single swap, it means the array is already sorted. We can then `break` out of the loop early.

### JavaScript Code

```javascript
function bubbleSortOptimized(arr) {
    let n = arr.length;
    
    for (let i = n - 1; i >= 1; i--) {
        let didSwap = false; 
        
        for (let j = 0; j <= i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                let temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                
                didSwap = true; 
            }
        }
        
        // If no elements were swapped, the array is sorted
        if (!didSwap) {
            break;
        }
    }
    return arr;
}

// Example Usage
const arr2 = [13, 46, 24, 52, 20, 9];
console.log("Optimized Sorted:", bubbleSortOptimized(arr2));
```

### Complexity
* **Time Complexity:** * **Worst & Average Case:** $O(N^2)$ (when the array is reverse sorted or randomly shuffled).
  * **Best Case:** $O(N)$ (when the array is already sorted, the outer loop runs once, checks the `didSwap` flag, and breaks).
* **Space Complexity:** $O(1)$ auxiliary space.