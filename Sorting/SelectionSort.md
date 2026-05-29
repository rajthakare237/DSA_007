# Selection Sort Algorithm in JavaScript 🎯

This repository contains the implementation of the **Selection Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Selection Sort?
Selection Sort is a simple and intuitive sorting algorithm. It divides the input list into two parts: a sorted sublist of items which is built up from left to right, and a sublist of the remaining unsorted items that occupy the rest of the list.

**Core Idea:** Repeatedly find the minimum element from the unsorted portion of the array and swap it with the first element of the unsorted portion. 

---

## 1️⃣ Standard / Optimal Approach

The standard implementation uses an outer loop to select the starting point of the unsorted array, and an inner loop to scan through the remaining elements to find the index of the absolute minimum value. Finally, it swaps the minimum value into its correct position.

### JavaScript Code

```javascript
/**
 * Standard Selection Sort
 * Repeatedly finds the minimum element from the unsorted part and puts it at the beginning.
 */
function selectionSort(arr) {
    let n = arr.length;
    
    // Outer loop bounds the unsorted subarray (goes up to the second-to-last element)
    for (let i = 0; i <= n - 2; i++) {
        let minIndex = i; // Assume the current index holds the minimum value
        
        // Inner loop finds the actual minimum in the remaining unsorted array
        for (let j = i + 1; j <= n - 1; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        
        // Swap the found minimum element with the first element of the unsorted part
        if (minIndex !== i) {
            let temp = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = temp;
        }
    }
    return arr;
}

// Example Usage
const arr1 = [13, 46, 24, 52, 20, 9];
console.log("Selection Sorted:", selectionSort(arr1));
```

### Complexity
* **Time Complexity:** $O(N^2)$ for all cases (Worst, Average, and Best). Unlike Bubble or Insertion Sort, Selection Sort does not adapt to the existing order of the array. It will always scan the entire remaining array to find the minimum element, even if the array is already sorted.
* **Space Complexity:** $O(1)$ auxiliary space, as it sorts in-place by swapping.