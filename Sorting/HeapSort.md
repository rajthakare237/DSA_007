# Heap Sort Algorithm in JavaScript 🌲

This repository contains the implementation of the **Heap Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Heap Sort?
Heap Sort is a popular and efficient comparison-based sorting algorithm that uses a **Binary Heap** data structure. It can be thought of as an improved Selection Sort; instead of doing a linear scan to find the maximum element, it uses a heap to find the maximum in $O(1)$ time and remove it in $O(\log N)$ time.

**Core Idea:** 1. **Build a Max-Heap:** Transform the unsorted array into a max-heap (a complete binary tree where every parent node is greater than its children).
2. **Extract Elements:** Repeatedly swap the root (the largest element) with the last element of the heap, reduce the heap size by one, and `heapify` the new root to maintain the max-heap property.

---

## 1️⃣ Standard / Optimal Approach

The implementation requires two functions: the main `heapSort` function that orchestrates the building and extracting, and a helper `heapify` function that ensures the sub-trees maintain the max-heap property.

### JavaScript Code

```javascript
/**
 * Helper function to maintain the max-heap property.
 * Compares the root node 'i' with its children and swaps if necessary.
 */
function heapify(arr, n, i) {
    let largest = i;          // Assume root is the largest
    let left = 2 * i + 1;     // Left child index
    let right = 2 * i + 2;    // Right child index

    // Check if left child exists and is greater than root
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }

    // Check if right child exists and is greater than the current largest
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }

    // If the largest is not the root, swap and recursively heapify the affected sub-tree
    if (largest !== i) {
        let temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;

        heapify(arr, n, largest);
    }
}

/**
 * Standard Heap Sort
 * Builds a max-heap, then extracts the maximum element one by one to the end of the array.
 */
function heapSort(arr) {
    let n = arr.length;

    // Step 1: Build a max-heap. 
    // Start from the last non-leaf node and heapify upwards.
    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }

    // Step 2: Extract elements from the heap one by one.
    for (let i = n - 1; i > 0; i--) {
        // Swap the current root (maximum element) with the end of the array
        let temp = arr[0];
        arr[0] = arr[i];
        arr[i] = temp;

        // Call heapify on the reduced heap to reposition the new root
        heapify(arr, i, 0);
    }
    return arr;
}

// Example Usage
const arr1 = [13, 46, 24, 52, 20, 9];
console.log("Heap Sorted:", heapSort(arr1));
```

### Complexity
* **Time Complexity:** $O(N \log N)$ for all cases (Worst, Average, and Best). 
  * Building the initial max-heap takes $O(N)$ time.
  * Extracting elements and calling `heapify` takes $O(\log N)$ time for each of the $N$ elements, resulting in $O(N \log N)$ time.
* **Space Complexity:** $O(1)$ auxiliary space, as the sorting is done entirely in-place (recursive call stack takes $O(\log N)$ space, but no extra arrays are created).