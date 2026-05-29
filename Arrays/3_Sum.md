# 3 Sum Problem in JavaScript 3️⃣

This repository contains the implementation of the **3 Sum** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the 3 Sum Problem?
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

**Core Constraint:** The solution set must not contain duplicate triplets. 

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to check every possible triplet using three nested loops. To ensure we don't include duplicate triplets, we sort the three elements and store them in a Set. 

*(Note: In JavaScript, objects/arrays are stored by reference, so we serialize them to strings to enforce uniqueness in the Set).*

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Uses three nested loops to check all triplets. 
 */
function threeSumBruteForce(arr) {
    let n = arr.length;
    let uniqueTriplets = new Set();
    
    for (let i = 0; i < n; i++) {
        for (let j = i + 1; j < n; j++) {
            for (let k = j + 1; k < n; k++) {
                if (arr[i] + arr[j] + arr[k] === 0) {
                    // Sort the triplet to ensure uniqueness regardless of original order
                    let temp = [arr[i], arr[j], arr[k]].sort((a, b) => a - b);
                    uniqueTriplets.add(JSON.stringify(temp)); 
                }
            }
        }
    }
    
    // Convert the Set of strings back into an array of arrays
    return Array.from(uniqueTriplets).map(JSON.parse);
}

// Example Usage
const arr1 = [-1, 0, 1, 2, -1, -4];
console.log("Brute Force 3 Sum:", threeSumBruteForce(arr1)); 
// Output: [[-1,-1,2], [-1,0,1]]
```

### Complexity
* **Time Complexity:** $O(N^3 \times \log(\text{size of set}))$ where $N$ is the number of elements. The nested loops take $N^3$ and inserting into a Set involves stringifying/comparing. This will definitely cause a Time Limit Exceeded (TLE) error.
* **Space Complexity:** $O(2 \times \text{number of unique triplets})$ for the Set and the return array.

---

## 2️⃣ Better Approach (Hashing)

We can reduce the time complexity by removing the third loop. We can use a Hash Set to look up the third element. Since $A + B + C = 0$, the third element we need is $C = -(A + B)$. As we iterate `j`, we check if this required element already exists in our Hash Set.

### JavaScript Code

```javascript
/**
 * Better Approach (Hashing)
 * Uses two loops and a HashSet to find the third required element.
 */
function threeSumBetter(arr) {
    let n = arr.length;
    let uniqueTriplets = new Set();
    
    for (let i = 0; i < n; i++) {
        // Create a new HashSet for the elements between i and j
        let hashSet = new Set();
        
        for (let j = i + 1; j < n; j++) {
            let needed = -(arr[i] + arr[j]);
            
            // If the required third element is in our hashSet, we found a triplet
            if (hashSet.has(needed)) {
                let temp = [arr[i], arr[j], needed].sort((a, b) => a - b);
                uniqueTriplets.add(JSON.stringify(temp));
            }
            
            // Add the current element to the hashSet for future lookups
            hashSet.add(arr[j]);
        }
    }
    
    return Array.from(uniqueTriplets).map(JSON.parse);
}

// Example Usage
const arr2 = [-1, 0, 1, 2, -1, -4];
console.log("Better 3 Sum (Hashing):", threeSumBetter(arr2));
```

### Complexity
* **Time Complexity:** $O(N^2 \times \log(\text{size of set}))$. We have eliminated the third loop, making it much faster.
* **Space Complexity:** $O(N)$ for the `hashSet` + $O(2 \times \text{number of unique triplets})$ to store the final answers. 

---

## 3️⃣ Optimal Approach (Sorting + Two Pointers)

We can optimize the space complexity by eliminating the Hash Set and the stringification Set. 
If we **sort the array first**, we can use the Two-Pointer technique. We fix one element `i`, and use two pointers (`j` at the start, `k` at the end) to find the remaining sum. 

Because the array is sorted, we can also easily **skip duplicate elements** on the fly, entirely removing the need for a Set to store unique triplets.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Two Pointers)
 * Sorts the array and uses two pointers to find pairs, skipping duplicates manually.
 */
function threeSumOptimal(arr) {
    let n = arr.length;
    let ans = [];
    
    // Step 1: Sort the array
    arr.sort((a, b) => a - b);
    
    for (let i = 0; i < n; i++) {
        // Skip duplicate elements for 'i' to avoid duplicate triplets
        if (i !== 0 && arr[i] === arr[i - 1]) continue;
        
        // Step 2: Use two pointers
        let j = i + 1;
        let k = n - 1;
        
        while (j < k) {
            let sum = arr[i] + arr[j] + arr[k];
            
            if (sum < 0) {
                j++; // We need a larger sum, move left pointer right
            } else if (sum > 0) {
                k--; // We need a smaller sum, move right pointer left
            } else {
                // We found a triplet that equals 0
                ans.push([arr[i], arr[j], arr[k]]);
                
                j++;
                k--;
                
                // Skip duplicate elements for 'j' and 'k'
                while (j < k && arr[j] === arr[j - 1]) j++;
                while (j < k && arr[k] === arr[k + 1]) k--;
            }
        }
    }
    
    return ans;
}

// Example Usage
const arr3 = [-1, 0, 1, 2, -1, -4];
console.log("Optimal 3 Sum (Two Pointers):", threeSumOptimal(arr3));
```

### Complexity
* **Time Complexity:** $O(N \log N)$ for sorting + $O(N^2)$ for the two-pointer traversal = roughly $O(N^2)$.
* **Space Complexity:** $O(1)$ auxiliary space (ignoring the space required for the output array), as we removed all sets and Hash Maps.