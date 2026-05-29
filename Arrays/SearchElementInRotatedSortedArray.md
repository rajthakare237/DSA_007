# Search in Rotated Sorted Array in JavaScript 🔍

This repository contains the implementation of the **Search Element in a Rotated Sorted Array** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given an integer array `nums` sorted in ascending order (with distinct values) that has been rotated at an unknown pivot index, and a `target` integer, return the **index** of `target` if it is in the array, or `-1` if it is not.

For example, `[0,1,2,4,5,6,7]` might be rotated to `[4,5,6,7,0,1,2]`. If we search for the target `0`, the algorithm should return index `4`.

---

## 1️⃣ Brute Force Approach

The simplest way to solve this is to ignore the fact that the array is rotated or sorted, and simply perform a **Linear Search**. We iterate through every element until we find the target.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Iterates through the entire array to find the target element.
 */
function searchBruteForce(arr, target) {
    let n = arr.length;
    
    for (let i = 0; i < n; i++) {
        if (arr[i] === target) {
            return i; // Return the index if found
        }
    }
    
    return -1; // Return -1 if the target is not in the array
}

// Example Usage
const arr1 = [4, 5, 6, 7, 0, 1, 2];
const target1 = 0;
console.log("Brute Force Search:", searchBruteForce(arr1, target1)); 
// Output: 4
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of elements in the array. In the worst case, we check every single element.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Optimal Approach (Binary Search)

To achieve $O(\log N)$ time complexity, we must use **Binary Search**. 

**The Logic:** 1. We divide the array using `mid`. 
2. In a rotated sorted array, **at least one half (left or right) will always be strictly sorted**.
3. We identify which half is sorted.
4. We check if our `target` falls within the range of that sorted half.
5. If it does, we eliminate the *unsorted* half. If it doesn't, we eliminate the *sorted* half.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Binary Search)
 * Identifies the sorted half and checks if the target lies within it to eliminate halves.
 */
function searchOptimal(arr, target) {
    let low = 0;
    let high = arr.length - 1;
    
    while (low <= high) {
        let mid = Math.floor((low + high) / 2);
        
        // Base condition: Target found
        if (arr[mid] === target) {
            return mid;
        }
        
        // Step 1: Check if the Left half is sorted
        if (arr[low] <= arr[mid]) {
            // Step 2: Check if target lies within this sorted left half
            if (arr[low] <= target && target <= arr[mid]) {
                high = mid - 1; // Target is here, eliminate right half
            } else {
                low = mid + 1;  // Target is NOT here, eliminate left half
            }
        } 
        // Step 3: Otherwise, the Right half must be sorted
        else {
            // Step 4: Check if target lies within this sorted right half
            if (arr[mid] <= target && target <= arr[high]) {
                low = mid + 1;  // Target is here, eliminate left half
            } else {
                high = mid - 1; // Target is NOT here, eliminate right half
            }
        }
    }
    
    return -1; // Target not found
}

// Example Usage
const arr2 = [4, 5, 6, 7, 0, 1, 2];
const target2 = 0;
console.log("Optimal Search (Binary Search):", searchOptimal(arr2, target2));
// Output: 4
```

### Complexity
* **Time Complexity:** $O(\log N)$ where $N$ is the size of the array. The search space is divided by half in every iteration, making it highly efficient.
* **Space Complexity:** $O(1)$ auxiliary space, as we are only updating pointers (`low`, `high`, `mid`).