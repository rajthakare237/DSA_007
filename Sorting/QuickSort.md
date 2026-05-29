# Quick Sort Algorithm in JavaScript ⚡

This repository contains the implementation of the **Quick Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Quick Sort?
Quick Sort is a highly efficient, comparison-based sorting algorithm that uses the **Divide and Conquer** strategy. It is generally preferred over Merge Sort for arrays because it does not require extra memory space (it sorts in-place).

**Core Idea:** 1. **Pick a Pivot:** Choose an element from the array as a pivot (in this implementation, the first element).
2. **Partition:** Rearrange the array so that all elements smaller than the pivot are placed to its left, and all elements greater are placed to its right. The pivot is now in its final sorted position.
3. **Recursion:** Recursively apply the same logic to the left and right sub-arrays.

---

## 1️⃣ Standard / Optimal Approach

The implementation uses three functions: the main wrapper function `quickSort`, the recursive `qs` helper function, and the `partition` function which does the actual heavy lifting of placing the pivot in its correct spot.

### JavaScript Code

```javascript
/**
 * Partition function
 * Places the pivot (first element) in its correct sorted position.
 * Moves smaller elements to the left and larger elements to the right.
 */
function partition(arr, low, high) {
    let pivot = arr[low]; 
    let i = low;
    let j = high;

    while (i < j) {
        // Move 'i' forward until we find an element greater than the pivot
        while (arr[i] <= pivot && i <= high - 1) {
            i++;
        }
        
        // Move 'j' backward until we find an element smaller than or equal to the pivot
        while (arr[j] > pivot && j >= low + 1) {
            j--;
        }
        
        // Swap elements at i and j if they haven't crossed
        if (i < j) {
            let temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }
    
    // Swap the pivot with arr[j] to place the pivot in its correct sorted position
    let temp = arr[low];
    arr[low] = arr[j];
    arr[j] = temp;

    return j; // Return the partition index
}

/**
 * Recursive Quick Sort helper function
 */
function qs(arr, low, high) {
    if (low < high) {
        let pIndex = partition(arr, low, high);
        
        // Recursively sort the left and right halves
        qs(arr, low, pIndex - 1);
        qs(arr, pIndex + 1, high);
    }
}

/**
 * Main wrapper function
 */
function quickSort(arr) {
    let n = arr.length;
    qs(arr, 0, n - 1);
    return arr;
}

// Example Usage
const arr1 = [4, 6, 2, 5, 7, 9, 1, 3];
console.log("Quick Sorted:", quickSort(arr1));
```

### Complexity
* **Time Complexity:** * **Best & Average Case:** $O(N \log N)$ (when the partition process always picks the middle element as pivot or close to it, dividing the array into two nearly equal halves).
  * **Worst Case:** $O(N^2)$ (when the array is already sorted or reverse sorted, and we pick the first or last element as the pivot, leading to highly unbalanced partitions).
* **Space Complexity:** $O(1)$ auxiliary space. However, it requires $O(\log N)$ auxiliary stack space for recursion in the best/average case, and $O(N)$ space in the worst case.