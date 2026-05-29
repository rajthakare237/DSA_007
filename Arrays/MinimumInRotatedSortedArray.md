# Minimum in Rotated Sorted Array in JavaScript 🔄

This repository contains the implementation of the **Minimum in Rotated Sorted Array** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Suppose an array of length `n` sorted in ascending order is **rotated** between `1` and `n` times. For example, the array `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`. 

Given the sorted rotated array `nums` of **unique** elements, return the minimum element of this array.

**Core Idea:** You need to find the "pivot" point where the rotation happened, as the smallest element will always sit right after the highest element. 

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to completely ignore the fact that the array is partially sorted and just do a standard linear search. We iterate through every element in the array and keep track of the minimum value found.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Iterates through the entire array to find the minimum element.
 */
function findMinBruteForce(arr) {
    let min = Infinity;
    
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] < min) {
            min = arr[i];
        }
    }
    
    return min;
}

// Example Usage
const arr1 = [4, 5, 6, 7, 0, 1, 2];
console.log("Brute Force Min:", findMinBruteForce(arr1)); 
// Output: 0
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of elements in the array. We visit every single element.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Optimal Approach (Binary Search)

Since the array was originally sorted, we can use **Binary Search** to find the minimum in logarithmic time. 

**The Logic:** 1. We divide the array into a left half and a right half using `mid`.
2. In a rotated sorted array, **at least one half will always be sorted**.
3. We identify the sorted half, take its lowest value (which is just its leftmost element), and compare it with our running minimum.
4. Once we've recorded the minimum from the sorted half, we can safely **eliminate** that entire half from our search space and move to the unsorted half.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Binary Search)
 * Identifies the sorted half, extracts its minimum, and searches the other half.
 */
function findMinOptimal(arr) {
    let low = 0;
    let high = arr.length - 1;
    let ans = Infinity;
    
    while (low <= high) {
        // Optimization: If the current search space is already fully sorted, 
        // the minimum is simply the first element (arr[low]).
        if (arr[low] <= arr[high]) {
            ans = Math.min(ans, arr[low]);
            break;
        }
        
        let mid = Math.floor((low + high) / 2);
        
        // Check if the Left half is sorted
        if (arr[low] <= arr[mid]) {
            // The minimum in the left half is arr[low]
            ans = Math.min(ans, arr[low]);
            
            // Eliminate the left half
            low = mid + 1; 
        } 
        // Otherwise, the Right half must be sorted
        else {
            // The minimum in the right half is arr[mid]
            ans = Math.min(ans, arr[mid]);
            
            // Eliminate the right half
            high = mid - 1; 
        }
    }
    
    return ans;
}

// Example Usage
const arr2 = [4, 5, 6, 7, 0, 1, 2];
console.log("Optimal Min (Binary Search):", findMinOptimal(arr2));
// Output: 0
```

### Complexity
* **Time Complexity:** $O(\log N)$ where $N$ is the size of the array. The search space is divided by half in every iteration.
* **Space Complexity:** $O(1)$ auxiliary space, as we only use a few pointer variables (`low`, `high`, `mid`, `ans`).