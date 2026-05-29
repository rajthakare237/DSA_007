# Merge Sort Algorithm in JavaScript 🔀

This repository contains the implementation of the **Merge Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Merge Sort?
Merge Sort is a highly efficient, general-purpose sorting algorithm based on the **Divide and Conquer** paradigm. 

**Core Idea:** 1. **Divide:** Recursively divide the array into two equal halves until each half contains only one element (a single element is inherently sorted).
2. **Merge:** Repeatedly combine (merge) the sorted halves back together to form the final sorted array.

---

## 1️⃣ Standard / Optimal Approach

The implementation relies on two main components:
* A recursive function (`mergeSortHelper`) that divides the array.
* A helper function (`merge`) that takes two sorted halves and combines them into a single sorted sequence using a temporary array.

### JavaScript Code

```javascript
/**
 * Merges two sorted halves of the array: [low...mid] and [mid+1...high]
 */
function merge(arr, low, mid, high) {
    let temp = []; // Temporary array to hold merged elements
    let left = low;      // Starting index of left half
    let right = mid + 1; // Starting index of right half

    // Compare and push the smaller element into temp
    while (left <= mid && right <= high) {
        if (arr[left] <= arr[right]) {
            temp.push(arr[left]);
            left++;
        } else {
            temp.push(arr[right]);
            right++;
        }
    }

    // Push any remaining elements from the left half
    while (left <= mid) {
        temp.push(arr[left]);
        left++;
    }

    // Push any remaining elements from the right half
    while (right <= high) {
        temp.push(arr[right]);
        right++;
    }

    // Transfer sorted elements from temp back to the original array
    for (let i = low; i <= high; i++) {
        arr[i] = temp[i - low];
    }
}

/**
 * Recursively divides the array into halves
 */
function mergeSortHelper(arr, low, high) {
    // Base case: If the array has 1 or 0 elements, it is already sorted
    if (low >= high) return;
    
    let mid = Math.floor((low + high) / 2);
    
    // Divide left and right halves
    mergeSortHelper(arr, low, mid);
    mergeSortHelper(arr, mid + 1, high);
    
    // Merge the sorted halves
    merge(arr, low, mid, high);
}

/**
 * Main wrapper function
 */
function mergeSort(arr) {
    let n = arr.length;
    mergeSortHelper(arr, 0, n - 1);
    return arr;
}

// Example Usage
const arr1 = [13, 46, 24, 52, 20, 9];
console.log("Merge Sorted:", mergeSort(arr1));
```

### Complexity
* **Time Complexity:** $O(N \log N)$ for all cases (Worst, Average, and Best). 
  * At each step, the array is divided into half ($\log N$ steps).
  * At each step, we merge elements which takes linear time ($N$). 
  * Therefore, $N \times \log N$.
* **Space Complexity:** $O(N)$ auxiliary space. The algorithm requires a temporary array (`temp`) of size $N$ to merge the sorted halves.