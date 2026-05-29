# Contains Duplicate in JavaScript 👯

This repository contains the implementation of the **Contains Duplicate** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Contains Duplicate Problem?
Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Core Idea:** You need to find if there are any repeating elements in the given array. 

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to take one element and compare it with all the other elements in the array using two nested loops.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Compares every single element with every other element.
 */
function containsDuplicateBruteForce(nums) {
    let n = nums.length;
    
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            // If we find a match, a duplicate exists
            if (nums[i] === nums[j]) {
                return true;
            }
        }
    }
    
    return false; // No duplicates found
}

// Example Usage
const nums1 = [1, 2, 3, 1];
console.log("Brute Force:", containsDuplicateBruteForce(nums1)); 
// Output: true
```

### Complexity
* **Time Complexity:** $O(N^2)$ where $N$ is the length of the array. We check every possible pair, which will result in a Time Limit Exceeded (TLE) error for large arrays.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Better Approach (Sorting)

We can optimize the search by sorting the array first. Once the array is sorted, any duplicate elements will be placed right next to each other. We can then do a single pass to check if any adjacent elements are the same.

### JavaScript Code

```javascript
/**
 * Better Approach (Sorting)
 * Sorts the array first, then checks adjacent elements.
 */
function containsDuplicateBetter(nums) {
    let n = nums.length;
    
    // Sort the array in ascending order
    nums.sort((a, b) => a - b);
    
    // Check if any adjacent elements are equal
    for (let i = 1; i < n; i++) {
        if (nums[i] === nums[i - 1]) {
            return true;
        }
    }
    
    return false;
}

// Example Usage
const nums2 = [1, 2, 3, 4];
console.log("Sorting (Better):", containsDuplicateBetter(nums2));
// Output: false
```

### Complexity
* **Time Complexity:** $O(N \log N)$ due to the sorting step. The subsequent linear scan takes $O(N)$ time, so the overall time is dominated by the sort.
* **Space Complexity:** $O(1)$ or $O(N)$ depending on the internal sorting algorithm used by JavaScript.

---

## 3️⃣ Optimal Approach (Hashing / Set)

The most optimal way to solve this is by using a **Hash Set**. A Set is a data structure that only stores unique values. As we iterate through the array, we check if the element is already in the Set. If it is, we found a duplicate. If not, we add it to the Set.

*(Note: In JavaScript, you can also solve this in one line by comparing the length of the array to the size of a newly created Set: `return new Set(nums).size !== nums.length;`)*

### JavaScript Code

```javascript
/**
 * Optimal Approach (Hash Set)
 * Uses a Set to keep track of elements we have already seen.
 */
function containsDuplicateOptimal(nums) {
    let seen = new Set();
    
    for (let i = 0; i < nums.length; i++) {
        // If the set already has the number, it's a duplicate
        if (seen.has(nums[i])) {
            return true;
        }
        
        // Otherwise, add it to our set of seen numbers
        seen.add(nums[i]);
    }
    
    return false;
}

// Example Usage
const nums3 = [1, 1, 1, 3, 3, 4, 3, 2, 4, 2];
console.log("Hash Set (Optimal):", containsDuplicateOptimal(nums3));
// Output: true
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of elements. We iterate through the array exactly once, and adding/checking elements in a Set takes $O(1)$ average time.
* **Space Complexity:** $O(N)$ auxiliary space, as in the worst-case scenario (no duplicates), we will store all $N$ elements in the Hash Set.