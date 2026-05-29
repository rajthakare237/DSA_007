# Container With Most Water in JavaScript 🌊

This repository contains the implementation of the **Container With Most Water** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the $i$-th line are `(i, 0)` and `(i, height[i])`. 

Find two lines that together with the x-axis form a container, such that the container contains the most water. Return the maximum amount of water a container can store.

**Core Idea:** The amount of water a container can hold is determined by its **width** (the distance between the two lines) multiplied by its **height** (the *shorter* of the two lines, because water will spill over the shorter side). 
Formula: `Area = width * min(height1, height2)`

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to check every possible pair of lines to see how much water they can trap, and keep track of the maximum volume found.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Checks every possible pair of lines using two nested loops.
 */
function maxAreaBruteForce(height) {
    let maxWater = 0;
    let n = height.length;
    
    // Outer loop picks the first line
    for (let i = 0; i < n; i++) {
        // Inner loop picks the second line
        for (let j = i + 1; j < n; j++) {
            
            // Calculate width and height
            let w = j - i;
            let h = Math.min(height[i], height[j]);
            
            let currentWater = w * h;
            
            // Update maxWater if current is larger
            if (currentWater > maxWater) {
                maxWater = currentWater;
            }
        }
    }
    
    return maxWater;
}

// Example Usage
const heights1 = [1, 8, 6, 2, 5, 4, 8, 3, 7];
console.log("Brute Force Max Water:", maxAreaBruteForce(heights1)); 
// Output: 49
```

### Complexity
* **Time Complexity:** $O(N^2)$ where $N$ is the number of elements in the array. We calculate the area for all possible pairs, which will result in a Time Limit Exceeded (TLE) error for large arrays.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Optimal Approach (Two Pointers)

We can optimize this to $O(N)$ using the **Two-Pointer** technique. 

**The Logic:** 1. We start with the widest possible container by placing one pointer at the beginning (`left = 0`) and one at the end (`right = n - 1`).
2. We calculate the area and update our maximum.
3. Now, we must move one pointer inward. Because the height of the water is bottlenecked by the *shorter* line, moving the taller line inward cannot possibly increase the area (the width is decreasing, and the height is still bottlenecked by the same short line). 
4. Therefore, we **always move the pointer pointing to the shorter line** inward in hopes of finding a taller line that compensates for the loss in width.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Two Pointers)
 * Starts with maximum width and moves the shorter line inward to maximize height.
 */
function maxAreaOptimal(height) {
    let maxWater = 0;
    let left = 0;
    let right = height.length - 1;
    
    while (left < right) {
        // Calculate the current area
        let w = right - left;
        let h = Math.min(height[left], height[right]);
        let currentWater = w * h;
        
        // Update maxWater
        if (currentWater > maxWater) {
            maxWater = currentWater;
        }
        
        // Move the pointer pointing to the shorter line
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxWater;
}

// Example Usage
const heights2 = [1, 8, 6, 2, 5, 4, 8, 3, 7];
console.log("Optimal Max Water (Two Pointers):", maxAreaOptimal(heights2));
// Output: 49
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of elements in the array. We iterate through the array exactly once as the two pointers move towards each other.
* **Space Complexity:** $O(1)$ auxiliary space, as we only use a few variables (`left`, `right`, `maxWater`).