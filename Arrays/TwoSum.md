# Two Sum Problem in JavaScript ➕

This repository contains the implementation of the **Two Sum Problem** in JavaScript, following the progression taught on [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Two Sum Problem?
Given an array of integers `arr` and an integer `target`, check if there exist two elements in the array such that their sum is equal to the `target`. 

There are usually two variations of this problem:
1. **Variety 1:** Return "YES" if the pair exists, otherwise return "NO".
2. **Variety 2:** Return the **indices** of the two numbers.

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to check every single pair in the array to see if they add up to the target sum. We use two nested loops for this.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Checks all possible pairs using nested loops.
 * Returns the indices of the elements that sum up to the target.
 */
function twoSumBruteForce(arr, target) {
    let n = arr.length;
    
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            if (arr[i] + arr[j] === target) {
                return [i, j];
            }
        }
    }
    return [-1, -1]; // Return -1, -1 if no pair is found
}

// Example Usage
const arr1 = [2, 6, 5, 8, 11];
const target1 = 14;
console.log("Brute Force:", twoSumBruteForce(arr1, target1));
```

### Complexity
* **Time Complexity:** $O(N^2)$ where $N$ is the size of the array. We are testing every combination.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Better Approach (Optimal for Returning Indices)

Instead of checking all pairs, we can use a Hash Map (or Object) to store the elements and their indices as we iterate through the array. For every element, we calculate its required `complement` (`target - current_element`) and check if it already exists in the map.

*Note: If the problem asks you to return **indices**, this is the absolute optimal approach.*

### JavaScript Code

```javascript
/**
 * Hashing Approach (Optimal for Indices)
 * Uses a Map to store elements and look up the required complement in O(1) time.
 */
function twoSumHashing(arr, target) {
    let map = new Map();
    
    for (let i = 0; i < arr.length; i++) {
        let complement = target - arr[i];
        
        // If the complement exists in the map, we found our pair!
        if (map.has(complement)) {
            return [map.get(complement), i];
        }
        
        // Otherwise, store the current element and its index
        map.set(arr[i], i);
    }
    return [-1, -1];
}

// Example Usage
const arr2 = [2, 6, 5, 8, 11];
const target2 = 14;
console.log("Hashing (Better):", twoSumHashing(arr2, target2));
```

### Complexity
* **Time Complexity:** $O(N)$ assuming the Map lookups and insertions take $O(1)$ time on average. 
* **Space Complexity:** $O(N)$ because in the worst-case scenario (no pair found until the end), we store all $N$ elements in the Hash Map.

---

## 3️⃣ Optimal Approach (For Returning "Yes/No")

If the problem only asks you to return **"YES/NO"** (or the elements themselves rather than their original indices), we can optimize the space complexity using the **Two-Pointer** technique. 

We first sort the array, place one pointer at the beginning and one at the end, and move them inwards based on whether the current sum is greater or smaller than the target.

### JavaScript Code

```javascript
/**
 * Two-Pointer Approach (Optimal for Yes/No)
 * Sorts the array first, then uses two pointers to find the sum.
 */
function twoSumTwoPointers(arr, target) {
    // Note: Sorting mutates the array and ruins original index positions
    arr.sort((a, b) => a - b); 
    
    let left = 0;
    let right = arr.length - 1;
    
    while (left < right) {
        let sum = arr[left] + arr[right];
        
        if (sum === target) {
            return "YES";
        } else if (sum < target) {
            left++; // We need a larger sum, move left pointer right
        } else {
            right--; // We need a smaller sum, move right pointer left
        }
    }
    return "NO";
}

// Example Usage
const arr3 = [2, 6, 5, 8, 11];
const target3 = 14;
console.log("Two Pointers (Optimal):", twoSumTwoPointers(arr3, target3));
```

### Complexity
* **Time Complexity:** $O(N \log N)$ mainly because sorting the array takes $N \log N$ time. The two-pointer traversal takes an additional $O(N)$ time.
* **Space Complexity:** $O(1)$ auxiliary space, as we do not use any extra data structures like hash maps.