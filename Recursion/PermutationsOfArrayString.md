# Permutations of an Array / String in JavaScript 🔀

This repository contains the implementation of the **Permutations** problem in JavaScript, following the Backtracking teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given an array `nums` of distinct integers (or a string of distinct characters), return all the possible **permutations**. You can return the answer in any order.

**Core Idea:** A permutation is an arrangement of all elements in a specific order. Mathematically, an array of $N$ distinct elements has exactly $N!$ (N factorial) permutations.
* Example: `nums = [1, 2, 3]`
* Output: `[[1,2,3], [1,3,2], [2,1,3], [2,3,1], [3,1,2], [3,2,1]]`

---

## 1️⃣ Approach 1: Recursive with Extra Space (Visited Array)

The most intuitive way to build permutations is to pick elements one by one from the array. To ensure we don't pick the same element twice in a single permutation, we maintain a boolean `visited` (or `freq`) array. 

**The Logic:**
1. Loop through all elements in the array.
2. If an element is not visited, mark it as visited, add it to our current permutation data structure, and recursively call the function for the next position.
3. Once the recursion returns, **backtrack** by unmarking the element and removing it from our data structure so it can be used in other permutations.

### JavaScript Code

```javascript
/**
 * Approach 1 (Visited Array)
 * Uses a boolean array to keep track of which elements have been picked.
 */
function permuteWithSpace(nums) {
    let ans = [];
    let visited = new Array(nums.length).fill(false);
    
    function generate(currentPerm) {
        // Base case: If the permutation length equals the array length
        if (currentPerm.length === nums.length) {
            ans.push([...currentPerm]); // Push a copy
            return;
        }
        
        // Try all elements
        for (let i = 0; i < nums.length; i++) {
            if (!visited[i]) {
                // Pick the element
                visited[i] = true;
                currentPerm.push(nums[i]);
                
                // Recurse
                generate(currentPerm);
                
                // Backtrack: Undo the choice
                visited[i] = false;
                currentPerm.pop();
            }
        }
    }
    
    generate([]);
    return ans;
}

// Example Usage
const nums1 = [1, 2, 3];
console.log("Visited Array Approach:", permuteWithSpace(nums1)); 
```

### Complexity
* **Time Complexity:** $O(N! \times N)$ where $N$ is the number of elements. We generate $N!$ permutations, and creating a copy of the array takes $O(N)$ time.
* **Space Complexity:** $O(N)$ for the `visited` array + $O(N)$ for the `currentPerm` array + $O(N)$ for the recursive call stack. Total auxiliary space is $O(N)$.

---

## 2️⃣ Optimal Approach: In-Place Swapping

We can eliminate the extra $O(N)$ space used by the `visited` array and the `currentPerm` array. Instead of building a new array, we can simply **swap** elements within the original array to create permutations in place.

**The Logic:**
1. We maintain a pointer `index`. Everything to the *left* of `index` is fixed.
2. We loop `i` from `index` to the end of the array.
3. We swap `nums[index]` with `nums[i]`, effectively placing a new element at the `index` position.
4. We recursively call the function for `index + 1`.
5. Upon returning, we **backtrack** by swapping the elements back to restore the original array order.

### JavaScript Code

```javascript
/**
 * Optimal Approach (In-Place Swapping)
 * Generates permutations by swapping elements directly in the input array.
 */
function permuteOptimal(nums) {
    let ans = [];
    
    function generate(index, arr) {
        // Base case: If we have fixed all positions
        if (index === arr.length) {
            ans.push([...arr]); 
            return;
        }
        
        // Swap the current index with every element from 'index' to the end
        for (let i = index; i < arr.length; i++) {
            // Pick: Swap to place element at 'index'
            [arr[index], arr[i]] = [arr[i], arr[index]];
            
            // Recurse for the next position
            generate(index + 1, arr);
            
            // Backtrack: Swap back to restore original state
            [arr[index], arr[i]] = [arr[i], arr[index]];
        }
    }
    
    generate(0, nums);
    return ans;
}

// Example Usage
const nums2 = [1, 2, 3];
console.log("In-Place Swapping Approach:", permuteOptimal(nums2));
```

### Complexity
* **Time Complexity:** $O(N! \times N)$ because generating $N!$ permutations and deep copying them takes linear time relative to the array size.
* **Space Complexity:** $O(1)$ auxiliary space (ignoring the recursion stack and output array). We no longer use extra frequency arrays.

---

## 3️⃣ Follow-Up: Permutations with Duplicates

**What if the input array contains duplicates (e.g., `[1, 1, 2]`)?** The swapping logic will generate identical permutations if we swap duplicate numbers into the same `index` position.

**The Fix:** We can use a local `Set` *inside* our recursive function. Before swapping an element into the `index` position, we check if we've already placed that exact number in this position during the current loop. If we have, we skip it.

### JavaScript Code

```javascript
/**
 * Permutations II (Handling Duplicates)
 * Uses a local Set at each recursive level to avoid redundant swaps.
 */
function permuteUnique(nums) {
    let ans = [];
    
    function generate(index, arr) {
        if (index === arr.length) {
            ans.push([...arr]);
            return;
        }
        
        // Local set to track which numbers have been placed at 'index'
        let seen = new Set();
        
        for (let i = index; i < arr.length; i++) {
            // If we've already swapped this value to 'index', skip it
            if (seen.has(arr[i])) continue;
            
            seen.add(arr[i]);
            
            // Swap, Recurse, Backtrack
            [arr[index], arr[i]] = [arr[i], arr[index]];
            generate(index + 1, arr);
            [arr[index], arr[i]] = [arr[i], arr[index]];
        }
    }
    
    generate(0, nums);
    return ans;
}

// Example Usage
const nums3 = [1, 1, 2];
console.log("Permutations with Duplicates:", permuteUnique(nums3));
// Output: [[1, 1, 2], [1, 2, 1], [2, 1, 1]]
```