# Combination Sum in JavaScript 🎯

This repository contains the implementation of the **Combination Sum** problem in JavaScript, following the Backtracking teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given an array of **distinct** integers `candidates` and a target integer `target`, return a list of all unique combinations of `candidates` where the chosen numbers sum to `target`. You may return the combinations in any order.

**Core Rule:** The same number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

* Example: `candidates = [2, 3, 6, 7]`, `target = 7`
* Output: `[[2, 2, 3], [7]]`

---

## 1️⃣ Optimal Approach (Recursion: Pick / Not Pick)

To generate combinations, we use the standard recursive **Pick / Not Pick** strategy. However, because we are allowed to use the exact same element multiple times, we slightly modify the rule for the "Pick" decision.

**The Logic:** We traverse the array element by element.
1. **Pick Decision:** If the current element is less than or equal to our remaining target, we add it to our subset and reduce the target. **Crucially, we do not move the index forward.** We recursively call the function staying on the exact same index `i`, allowing the program to pick it again.
2. **Not Pick Decision:** We simply ignore the current element and move to the next index (`i + 1`) without changing the target.
3. **Base Case:** If our index falls off the array, we check if our remaining target has hit exactly `0`. If it has, we found a valid combination.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Backtracking)
 * Uses the Pick / Not Pick strategy, staying on the same index when picking.
 */
function combinationSum(candidates, target) {
    let ans = [];
    
    function findCombinations(index, currentTarget, currentCombo) {
        // Base case: If we have considered all elements
        if (index === candidates.length) {
            // If we successfully hit the target, save this combination
            if (currentTarget === 0) {
                ans.push([...currentCombo]);
            }
            return;
        }
        
        // Decision 1: PICK the element
        // We can only pick it if it doesn't exceed our remaining target
        if (candidates[index] <= currentTarget) {
            currentCombo.push(candidates[index]);
            
            // Recurse with reduced target, but STAY on the same 'index'
            findCombinations(index, currentTarget - candidates[index], currentCombo);
            
            // Backtrack: Remove the element to explore other branches
            currentCombo.pop();
        }
        
        // Decision 2: NOT PICK the element
        // Move to the next element (index + 1) without changing the target
        findCombinations(index + 1, currentTarget, currentCombo);
    }
    
    // Start recursion from index 0, with the initial target, and an empty combo array
    findCombinations(0, target, []);
    return ans;
}

// Example Usage
const candidates1 = [2, 3, 6, 7];
const target1 = 7;
console.log("Combinations:", combinationSum(candidates1, target1)); 
// Output: [[2, 2, 3], [7]]
```

### Complexity
* **Time Complexity:** $O(2^T \times K)$ where $T$ is the target value and $K$ is the average length of every combination. Because we can pick elements unlimited times, the recursion tree doesn't stop exactly at $N$ depth; it stops when the target is reached. Copying the combination array takes $O(K)$ time.
* **Space Complexity:** $O(T/M)$ auxiliary space, where $M$ is the minimum element in the array. This is because the maximum depth of the recursive call stack occurs when we repeatedly pick the smallest element until we reach the target (e.g., target 10, smallest element 2, depth = 5). We also use temporary space for the `currentCombo` array during recursion.