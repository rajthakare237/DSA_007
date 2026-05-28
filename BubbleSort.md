
# Bubble Sort in JavaScript

This document contains implementations of the Bubble Sort algorithm in JavaScript, progressing from a brute-force approach to the most optimal solution. 

## 1. Brute Force Solution

This basic implementation repeatedly steps through the list, compares adjacent elements, and swaps them if they are in the wrong order. It always runs in O(N²) time, regardless of whether the array is already sorted.

```javascript
// Always runs O(N^2) times, even if the array is sorted.
function bubbleSortBrute(arr) {
  let n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  return arr;
}
```

## 2. Optimized Solution (Early Exit)

By introducing a `swapped` flag, the algorithm stops early if no swaps occur during an entire pass. This improves the best-case time complexity to O(N) when the array is already sorted.

```javascript
// Stops early if the array becomes completely sorted.
function bubbleSortOptimized(arr) {
  let n = arr.length;
  let swapped;
  for (let i = 0; i < n - 1; i++) {
    swapped = false;
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
        swapped = true;
      }
    }
    if (!swapped) break;
  }
  return arr;
}
```

## 3. Fully Optimized Solution

This version goes a step further by keeping track of the exact index of the last swap. Any elements beyond the last swap are already sorted in their correct final positions, so the inner loop boundary can be drastically reduced on subsequent passes.

```javascript
// Skips elements that are already in their correct sorted positions.
function bubbleSortFullyOptimized(arr) {
  let n = arr.length;
  while (n > 1) {
    let newN = 0;
    for (let i = 1; i < n; i++) {
      if (arr[i - 1] > arr[i]) {
        [arr[i - 1], arr[i]] = [arr[i], arr[i - 1]];
        newN = i; // Mark the position of the last swap
      }
    }
    n = newN; // Shrink the boundary for the next pass
  }
  return arr;
}
```
