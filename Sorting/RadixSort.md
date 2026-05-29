# Radix Sort Algorithm in JavaScript 🧮

This repository contains the implementation of the **Radix Sort** algorithm in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is Radix Sort?
Radix Sort is a non-comparison-based sorting algorithm that sorts elements digit by digit, starting from the least significant digit (ones place) to the most significant digit. 

**Core Idea:** Because elements are sorted digit by digit, Radix Sort requires a **stable** sorting algorithm as a subroutine to sort the digits at each place value. **Counting Sort** is almost exclusively used for this purpose. The algorithm will run the Counting Sort subroutine as many times as there are digits in the maximum number in the array.

---

## 1️⃣ Standard / Optimal Approach

This implementation finds the maximum number to determine the number of digits. It then uses a modified Counting Sort function that specifically looks at a single digit (determined by an exponent `exp` which iterates as 1, 10, 100, etc.) rather than the entire number.

### JavaScript Code

```javascript
/**
 * A modified Counting Sort specifically designed to act as a stable
 * subroutine for Radix Sort. It sorts based on the digit represented by 'exp'.
 */
function countingSortForRadix(arr, exp) {
    let n = arr.length;
    let output = new Array(n);
    
    // There are only 10 possible digits (0-9) in base 10
    let count = new Array(10).fill(0);

    // Step 1: Store the frequency of the targeted digit
    for (let i = 0; i < n; i++) {
        let digit = Math.floor(arr[i] / exp) % 10;
        count[digit]++;
    }

    // Step 2: Compute cumulative counts to find positions
    for (let i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }

    // Step 3: Build the output array (iterating backwards for stability)
    for (let i = n - 1; i >= 0; i--) {
        let digit = Math.floor(arr[i] / exp) % 10;
        output[count[digit] - 1] = arr[i];
        count[digit]--;
    }

    // Step 4: Copy the sorted output back to the original array
    for (let i = 0; i < n; i++) {
        arr[i] = output[i];
    }
}

/**
 * Standard Radix Sort
 * Sorts an array of non-negative integers digit by digit.
 */
function radixSort(arr) {
    let n = arr.length;
    if (n <= 1) return arr;

    // Step 1: Find the maximum number to know the maximum number of digits
    let max = arr[0];
    for (let i = 1; i < n; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }

    // Step 2: Do counting sort for every digit.
    // 'exp' is 10^i where i is current digit number (1, 10, 100...)
    for (let exp = 1; Math.floor(max / exp) > 0; exp *= 10) {
        countingSortForRadix(arr, exp);
    }

    return arr;
}

// Example Usage (Non-negative integers)
const arr1 = [170, 45, 75, 90, 802, 24, 2, 66];
console.log("Radix Sorted:", radixSort(arr1));
```

### Complexity
* **Time Complexity:** $O(d \times (N + b))$ for all cases (Worst, Average, and Best). 
  * $N$ is the number of elements.
  * $b$ is the base representing numbers (for decimal numbers, $b = 10$).
  * $d$ is the number of digits in the maximum element.
  * The algorithm runs $d$ passes of counting sort, and each pass takes $O(N + b)$ time.
* **Space Complexity:** $O(N + b)$ auxiliary space. The Counting Sort subroutine requires an `output` array of size $N$ and a `count` array of size $b$ (which is 10).