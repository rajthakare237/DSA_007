# Product of Array Except Itself in JavaScript ✖️

This repository contains the implementation of the **Product of Array Except Itself** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given an integer array `nums`, return an array `ans` such that `ans[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

**Core Constraint:** You must write an algorithm that runs in $O(N)$ time and **without using the division operation**.

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to use two nested loops. For every element at index `i`, we iterate through the entire array again using index `j` and multiply the elements together as long as `i !== j`.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Calculates the product for each element using nested loops.
 */
function productExceptSelfBruteForce(nums) {
    let n = nums.length;
    let ans = new Array(n).fill(1);
    
    for (let i = 0; i < n; i++) {
        for (let j = 0; j < n; j++) {
            if (i !== j) {
                ans[i] *= nums[j];
            }
        }
    }
    
    return ans;
}

// Example Usage
const nums1 = [1, 2, 3, 4];
console.log("Brute Force:", productExceptSelfBruteForce(nums1)); 
// Output: [24, 12, 8, 6]
```

### Complexity
* **Time Complexity:** $O(N^2)$ where $N$ is the size of the array. For large arrays, this will result in a Time Limit Exceeded (TLE) error.
* **Space Complexity:** $O(1)$ auxiliary space (we do not count the output array `ans` as extra space).

---

## 2️⃣ Better Approach (Prefix and Suffix Arrays)

To achieve $O(N)$ time complexity, we can use two extra arrays: `prefix` and `suffix`. 
* `prefix[i]` will store the product of all elements to the **left** of `i`.
* `suffix[i]` will store the product of all elements to the **right** of `i`.
Multiplying these two values gives the product of all elements except the current one.

### JavaScript Code

```javascript
/**
 * Better Approach
 * Uses prefix and suffix arrays to precompute left and right products.
 */
function productExceptSelfBetter(nums) {
    let n = nums.length;
    let prefix = new Array(n).fill(1);
    let suffix = new Array(n).fill(1);
    let ans = new Array(n);
    
    // Step 1: Calculate prefix products
    for (let i = 1; i < n; i++) {
        prefix[i] = prefix[i - 1] * nums[i - 1];
    }
    
    // Step 2: Calculate suffix products
    for (let i = n - 2; i >= 0; i--) {
        suffix[i] = suffix[i + 1] * nums[i + 1];
    }
    
    // Step 3: Multiply prefix and suffix for the final answer
    for (let i = 0; i < n; i++) {
        ans[i] = prefix[i] * suffix[i];
    }
    
    return ans;
}

// Example Usage
const nums2 = [1, 2, 3, 4];
console.log("Better (Prefix/Suffix):", productExceptSelfBetter(nums2));
// Output: [24, 12, 8, 6]
```

### Complexity
* **Time Complexity:** $O(N)$ because we are making three separate linear passes through the array.
* **Space Complexity:** $O(N)$ auxiliary space because we created two extra arrays (`prefix` and `suffix`) of size $N$.

---

## 3️⃣ Optimal Approach (Space Optimized)

We can optimize the space complexity to $O(1)$ by eliminating the `prefix` and `suffix` arrays. Instead, we can use our final `ans` array to store the prefix products. Then, we can iterate backward, keeping track of a running `suffix` product variable, and multiply it directly into the `ans` array on the fly.

### JavaScript Code

```javascript
/**
 * Optimal Approach
 * Computes the answer in O(N) time and O(1) auxiliary space.
 */
function productExceptSelfOptimal(nums) {
    let n = nums.length;
    let ans = new Array(n).fill(1);
    
    // Step 1: Calculate prefix products directly into the 'ans' array
    let prefix = 1;
    for (let i = 0; i < n; i++) {
        ans[i] = prefix;
        prefix *= nums[i];
    }
    
    // Step 2: Traverse backwards, keeping a running suffix product, 
    // and multiply it directly into the 'ans' array
    let suffix = 1;
    for (let i = n - 1; i >= 0; i--) {
        ans[i] *= suffix;
        suffix *= nums[i];
    }
    
    return ans;
}

// Example Usage
const nums3 = [-1, 1, 0, -3, 3];
console.log("Optimal (Space Optimized):", productExceptSelfOptimal(nums3));
// Output: [0, 0, 9, 0, 0]
```

### Complexity
* **Time Complexity:** $O(N)$ because we make two linear passes through the array.
* **Space Complexity:** $O(1)$ auxiliary space. The `ans` array does not count towards extra space complexity in this problem.