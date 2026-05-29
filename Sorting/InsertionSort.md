# Insertion Sort Algorithm in JavaScript 🃏

This repository contains the implementation of the **Insertion Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Insertion Sort?
Insertion Sort is a simple sorting algorithm that builds the final sorted array one item at a time. It works similarly to the way you might sort playing cards in your hands. 

**Core Idea:** You take an element from the unsorted part of the array and place it in its correct order within the already sorted part of the array on the left.

---

## 1️⃣ Standard / Optimal Approach

The standard implementation uses an outer loop to pick the current element and an inner `while` loop to shift it to the left until it sits in its correct sorted position. 

### JavaScript Code

```javascript
/**
 * Standard Insertion Sort
 * Places the current element into its correct position in the sorted left half.
 */
function insertionSort(arr) {
    let n = arr.length;
    
    // Outer loop selects the element to be inserted
    for (let i = 0; i <= n - 1; i++) {
        let j = i;
        
        // Inner loop swaps the element to the left until it's in the right spot
        while (j > 0 && arr[j - 1] > arr[j]) {
            let temp = arr[j - 1];
            arr[j - 1] = arr[j];
            arr[j] = temp;
            
            j--; // Move backwards
        }
    }
    return arr;
}

// Example Usage
const arr1 = [13, 46, 24, 52, 20, 9];
console.log("Insertion Sorted:", insertionSort(arr1));
```

### Complexity
* **Time Complexity:** * **Worst & Average Case:** $O(N^2)$ (when the array is reverse sorted or randomly shuffled, forcing the inner loop to travel all the way back).
  * **Best Case:** $O(N)$ (when the array is already sorted. The condition `arr[j - 1] > arr[j]` instantly fails, so the inner `while` loop never runs, acting as a built-in optimization).
* **Space Complexity:** $O(1)$ auxiliary space, as it sorts in-place.