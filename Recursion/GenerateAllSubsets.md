# Generate All Subsets (Power Set) in JavaScript 🔢

This repository contains the implementation of the **Generate All Subsets (Power Set)** problem in JavaScript, following the Recursion teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given an integer array `nums` of **unique** elements, return all possible subsets (the power set). 

The solution set must not contain duplicate subsets. Return the solution in any order.
* Example: `nums = [1, 2, 3]`
* Output: `[[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]`

**Core Idea:** A set of $N$ elements will always have exactly $2^N$ possible subsets (including the empty set).

---

## 1️⃣ Optimal Approach 1 (Recursion: Pick / Not Pick)

The standard and most crucial way to solve this is using the recursive **Pick / Not Pick** (or Take / Not Take) pattern. 

**The Logic:** We traverse the array element by element. For every single element at index `i`, we make a branching decision:
1. **Not Pick:** We move to the next element without adding `nums[i]` to our current subset.
2. **Pick:** We add `nums[i]` to our current subset, and then move to the next element.
When our index reaches the end of the array, the current subset is a valid combination, and we add it to our final answer.

### JavaScript Code

```javascript
/**
 * Recursive Approach (Pick / Not Pick)
 * Uses backtracking to explore all combinations of taking or ignoring elements.
 */
function subsetsRecursive(nums) {
    let ans = [];
    
    // Helper function for recursion
    function generate(index, currentSubset) {
        // Base case: If we have considered all elements
        if (index === nums.length) {
            // Push a copy of the current subset to our answer array
            ans.push([...currentSubset]);
            return;
        }
        
        // Decision 1: NOT PICK the current element
        generate(index + 1, currentSubset);
        
        // Decision 2: PICK the current element
        currentSubset.push(nums[index]);
        generate(index + 1, currentSubset);
        
        // Backtrack: Remove the picked element so it doesn't affect other branches
        currentSubset.pop();
    }
    
    // Start recursion from index 0 with an empty subset
    generate(0, []);
    return ans;
}

// Example Usage
const nums1 = [1, 2, 3];
console.log("Recursive (Pick/Not Pick):", subsetsRecursive(nums1)); 
```

### Complexity
* **Time Complexity:** $O(2^N \times N)$ where $N$ is the number of elements. We generate $2^N$ subsets, and copying each subset (using `[...currentSubset]`) takes up to $O(N)$ time.
* **Space Complexity:** $O(N)$ auxiliary space for the recursive call stack and the `currentSubset` array. (Note: Storing the final answer takes $O(2^N \times N)$ space, but this is usually excluded from auxiliary space analysis).

---

## 2️⃣ Optimal Approach 2 (Bit Manipulation)

Another incredibly elegant way to solve this is by using **Bitwise Operations**. 

**The Logic:** If we have an array of length 3, we have $2^3 = 8$ subsets. If we write out the binary representation of the numbers $0$ to $7$, we get:
* `000` (Empty subset)
* `001` (Pick `nums[2]`)
* `010` (Pick `nums[1]`)
* `011` (Pick `nums[1]`, `nums[2]`)
* ...
* `111` (Pick `nums[0]`, `nums[1]`, `nums[2]`)

By looping from $0$ to $2^N - 1$, we can use the set bits (the `1`s) of each number to determine exactly which array elements to include in the current subset.

### JavaScript Code

```javascript
/**
 * Bit Manipulation Approach
 * Uses the binary representation of numbers from 0 to (2^N - 1) to build subsets.
 */
function subsetsBitwise(nums) {
    let n = nums.length;
    let subsetsCount = 1 << n; // 1 << n is mathematically equal to 2^n
    let ans = [];
    
    // Loop through all numbers from 0 to (2^n - 1)
    for (let num = 0; num < subsetsCount; num++) {
        let currentSubset = [];
        
        // Check all 'n' bits of the current number
        for (let i = 0; i < n; i++) {
            // If the i-th bit of 'num' is set to 1, pick the i-th element
            if ((num & (1 << i)) !== 0) {
                currentSubset.push(nums[i]);
            }
        }
        
        ans.push(currentSubset);
    }
    
    return ans;
}

// Example Usage
const nums2 = [1, 2, 3];
console.log("Bit Manipulation:", subsetsBitwise(nums2));
```

### Complexity
* **Time Complexity:** $O(2^N \times N)$. The outer loop runs $2^N$ times, and the inner loop checks $N$ bits for every single number.
* **Space Complexity:** $O(1)$ auxiliary space. Unlike recursion, we do not use any extra stack memory. We iteratively build the subsets directly into the answer array.