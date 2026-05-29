# Jump Game in JavaScript 🦘

This repository contains the implementation of the **Jump Game** problem in JavaScript, following the Greedy Algorithms teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your **maximum** jump length at that position.

Return `true` if you can reach the last index, or `false` otherwise.

* Example 1: `nums = [2, 3, 1, 1, 4]`
  * Output: `true` (Jump 1 step from index 0 to 1, then 3 steps to the last index.)
* Example 2: `nums = [3, 2, 1, 0, 4]`
  * Output: `false` (You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.)

---

## 1️⃣ Brute Force Approach (Recursion / Backtracking)

The most naive way to solve this is to try every single possible jump from the first index to the last. At any given index `i`, we can make jumps ranging from `1` to `nums[i]`. We can use recursion to explore all these branches.

### JavaScript Code

```javascript
/**
 * Brute Force Approach (Recursion)
 * Explores all possible jump lengths from every index to see if ANY path reaches the end.
 */
function canJumpBruteForce(nums) {
    
    function jumpFromPosition(position) {
        // Base case: If we have reached or passed the last index, success!
        if (position >= nums.length - 1) {
            return true;
        }
        
        // Calculate the furthest we can jump from the current position
        let furthestJump = Math.min(position + nums[position], nums.length - 1);
        
        // Try all possible jumps from 'position + 1' up to 'furthestJump'
        for (let nextPosition = position + 1; nextPosition <= furthestJump; nextPosition++) {
            // If any recursive path returns true, the game is winnable
            if (jumpFromPosition(nextPosition)) {
                return true;
            }
        }
        
        // If we exhausted all jumps and none reached the end, return false
        return false;
    }
    
    // Start the game from index 0
    return jumpFromPosition(0);
}

// Example Usage
const nums1 = [2, 3, 1, 1, 4];
console.log("Brute Force (Can Jump?):", canJumpBruteForce(nums1)); 
// Output: true
```

### Complexity
* **Time Complexity:** $O(2^N)$ in the worst case (though often represented as $O(N^N)$ depending on the upper bound of the values in the array). The recursion branches out heavily. This will cause a Time Limit Exceeded (TLE) error on large arrays.
* **Space Complexity:** $O(N)$ auxiliary space strictly for the maximum depth of the recursive call stack.

---

## 2️⃣ Optimal Approach (Greedy Algorithm)

We do not need to explore every single path. Instead, we can simply keep track of the **furthest index** we are currently able to reach. 

**The Logic:**
1. Maintain a variable `maxReach` initialized to 0.
2. Iterate through the array sequentially.
3. At any step `i`, if `i` is greater than `maxReach`, it means we are stuck! We could not even reach our current position from any of the previous steps. Return `false`.
4. Otherwise, update `maxReach` to be the maximum of our current `maxReach` or `i + nums[i]` (our current index + the jump power we have here).
5. If `maxReach` is ever greater than or equal to the last index, we can guarantee a win. Return `true`.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Greedy Algorithm)
 * Maintains a 'maxReach' boundary and updates it as we iterate forward.
 */
function canJumpOptimal(nums) {
    let maxReach = 0;
    
    for (let i = 0; i < nums.length; i++) {
        // If our current index is beyond our maximum reach, we are stuck!
        if (i > maxReach) {
            return false;
        }
        
        // Update our maximum reach if jumping from here takes us further
        maxReach = Math.max(maxReach, i + nums[i]);
        
        // Optimization: If we can already reach the end, no need to keep checking
        if (maxReach >= nums.length - 1) {
            return true;
        }
    }
    
    return true;
}

// Example Usage
const nums2 = [3, 2, 1, 0, 4];
console.log("Optimal Greedy (Can Jump?):", canJumpOptimal(nums2));
// Output: false
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the length of the array. We only make a single linear pass through the array.
* **Space Complexity:** $O(1)$ auxiliary space. We only use a single integer variable (`maxReach`), making it extremely memory efficient.