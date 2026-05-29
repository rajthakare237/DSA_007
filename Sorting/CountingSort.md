# Counting Sort Algorithm in JavaScript 🔢

This repository contains the implementation of the **Counting Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Counting Sort?
Counting Sort is a non-comparison-based sorting algorithm that sorts elements by counting the number of occurrences of each unique element in the array. It operates optimally when the difference between different keys (elements) is not significantly greater than the number of elements.

**Core Idea:** 1. Find the maximum element to determine the range.
2. Count the frequency of each element in a separate `count` array.
3. Modify the `count` array to store the prefix sums (which tells us the exact position of each element).
4. Iterate through the original array backwards (to maintain stability) and place elements in their correct sorted position in an `output` array.

---

## 1️⃣ Standard / Optimal Approach

This implementation assumes the array consists of non-negative integers. (Note: To handle negative numbers, you would also need to find the minimum element and shift the counting logic by the absolute value of the minimum).

### JavaScript Code

```javascript
/**
 * Standard Counting Sort
 * Sorts an array by counting element frequencies and computing their starting positions.
 */
function countingSort(arr) {
    let n = arr.length;
    if (n <= 1) return arr;

    // Step 1: Find the maximum element to size the count array
    let max = arr[0];
    for (let i = 1; i < n; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }

    // Step 2: Initialize count array with zeros
    let count = new Array(max + 1).fill(0);

    // Step 3: Store the count/frequency of each element
    for (let i = 0; i < n; i++) {
        count[arr[i]]++;
    }

    // Step 4: Modify the count array to store cumulative sums 
    // (This determines the actual position of the element in the sorted array)
    for (let i = 1; i <= max; i++) {
        count[i] += count[i - 1];
    }

    // Step 5: Build the output array
    // We iterate backwards to maintain the "stability" of the sorting algorithm
    let output = new Array(n);
    for (let i = n - 1; i >= 0; i--) {
        output[count[arr[i]] - 1] = arr[i];
        count[arr[i]]--; // Decrease count for duplicate elements
    }

    // Step 6: Copy the sorted elements back to the original array
    for (let i = 0; i < n; i++) {
        arr[i] = output[i];
    }

    return arr;
}

// Example Usage (Non-negative integers)
const arr1 = [4, 2, 2, 8, 3, 3, 1];
console.log("Counting Sorted:", countingSort(arr1));
```

### Complexity
* **Time Complexity:** $O(N + K)$ for all cases (Worst, Average, and Best), where $N$ is the number of elements in the input array and $K$ is the maximum value in the array. 
  * The algorithm does not compare elements. It iterates through the input array of size $N$ and the count array of size $K$.
* **Space Complexity:** $O(N + K)$ auxiliary space. It requires an `output` array of size $N$ and a `count` array of size $K + 1$. 
  * *Warning:* If $K$ (the maximum element) is astronomically large (e.g., sorting `[1, 1000000]`), Counting Sort becomes highly inefficient in terms of memory compared to comparison-based sorts.