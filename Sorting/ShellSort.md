# Shell Sort Algorithm in JavaScript 🐚

This repository contains the implementation of the **Shell Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Shell Sort?
Shell Sort is an in-place comparison sorting algorithm that serves as a highly optimized variation of Insertion Sort. Instead of only comparing adjacent elements, it compares elements separated by a specific "gap." 

**Core Idea:** The algorithm starts by sorting elements far apart from each other and progressively reduces the gap between elements. By the time the gap reduces to `1` (which acts exactly like a standard Insertion Sort), the array is nearly sorted, making the final pass extremely fast.

---

## 1️⃣ Standard / Optimal Approach

The standard implementation uses a gap sequence. The most common sequence (devised by Donald Shell) starts with a gap of $N/2$ and halves it in every iteration until the gap reaches $1$. 

### JavaScript Code

```javascript
/**
 * Standard Shell Sort
 * Uses a diminishing gap sequence to partially sort the array before a final insertion sort pass.
 */
function shellSort(arr) {
    let n = arr.length;
    
    // Start with a large gap, then reduce the gap by half each time
    for (let gap = Math.floor(n / 2); gap > 0; gap = Math.floor(gap / 2)) {
        
        // Perform a gapped insertion sort for the current gap size
        for (let i = gap; i < n; i++) {
            let temp = arr[i]; // The element to be inserted
            let j;
            
            // Shift earlier gap-sorted elements up until the correct location for 'temp' is found
            for (j = i; j >= gap && arr[j - gap] > temp; j -= gap) {
                arr[j] = arr[j - gap];
            }
            
            // Put temp (the original arr[i]) in its correct position
            arr[j] = temp;
        }
    }
    return arr;
}

// Example Usage
const arr1 = [13, 46, 24, 52, 20, 9];
console.log("Shell Sorted:", shellSort(arr1));
```

### Complexity
* **Time Complexity:** * **Worst Case:** $O(N^2)$ when using the original $N/2$ gap sequence. (Note: Using optimal gap sequences like Hibbard's or Sedgewick's can reduce the worst-case to $O(N^{1.5})$ or $O(N \log^2 N)$).
  * **Average Case:** Highly dependent on the gap sequence, but generally much faster than standard $O(N^2)$ algorithms, often approximated around $O(N \log N)$ or $O(N^{1.5})$.
  * **Best Case:** $O(N \log N)$ when the array is already sorted.
* **Space Complexity:** $O(1)$ auxiliary space, as it sorts in-place.