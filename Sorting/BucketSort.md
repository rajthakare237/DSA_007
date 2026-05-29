# Bucket Sort Algorithm in JavaScript 🪣

This repository contains the implementation of the **Bucket Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Bucket Sort?
Bucket Sort is a distribution sorting algorithm that works exceptionally well when the input data is uniformly distributed over a range. 

**Core Idea:** 1. **Scatter:** Create an array of empty "buckets" and distribute the elements into these buckets based on their mathematical value.
2. **Sort:** Sort each individual bucket (often using Insertion Sort or the language's built-in sort).
3. **Gather:** Concatenate the sorted buckets back together to form the final sorted array.

---

## 1️⃣ Standard / Optimal Approach

This implementation demonstrates Bucket Sort on an array of floating-point numbers in the range `[0, 1)`. The index of the bucket for each element is calculated by multiplying the element by the total number of elements `n`.

### JavaScript Code

```javascript
/**
 * Standard Bucket Sort
 * Distributes elements into buckets, sorts them individually, and concatenates them.
 */
function bucketSort(arr) {
    let n = arr.length;
    if (n <= 0) return arr;

    // Step 1: Create 'n' empty buckets
    let buckets = Array.from({ length: n }, () => []);

    // Step 2: Put array elements into different buckets
    for (let i = 0; i < n; i++) {
        // Calculate bucket index (specifically for numbers in range [0, 1))
        let bucketIndex = Math.floor(n * arr[i]);
        buckets[bucketIndex].push(arr[i]);
    }

    // Step 3: Sort individual buckets 
    // (Using native .sort() here, but Insertion Sort is also common)
    for (let i = 0; i < n; i++) {
        buckets[i].sort((a, b) => a - b);
    }

    // Step 4: Gather all elements back into the original array
    let index = 0;
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < buckets[i].length; j++) {
            arr[index] = buckets[i][j];
            index++;
        }
    }
    
    return arr;
}

// Example Usage (Uniformly distributed numbers between 0 and 1)
const arr1 = [0.897, 0.565, 0.656, 0.1234, 0.665, 0.3434];
console.log("Bucket Sorted:", bucketSort(arr1));
```

### Complexity
* **Time Complexity:** * **Average & Best Case:** $O(N + K)$ where $N$ is the number of elements and $K$ is the number of buckets. If the data is uniformly distributed, each bucket contains very few elements, making the sorting step nearly $O(1)$ per bucket.
  * **Worst Case:** $O(N^2)$ (or $O(N \log N)$ depending on the inner sorting algorithm used). This happens if the data is highly clustered and all elements get pushed into a single bucket.
* **Space Complexity:** $O(N + K)$ auxiliary space to store the $K$ buckets and the $N$ elements within them.