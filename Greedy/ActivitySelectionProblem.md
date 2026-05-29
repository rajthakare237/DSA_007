# Activity Selection Problem in JavaScript 📅

This repository contains the implementation of the **Activity Selection Problem** (also known as N-Meetings in One Room) in JavaScript, following the Greedy Algorithms teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
You are given $N$ activities with their start and end times. Select the maximum number of activities that can be performed by a single person, assuming that a person can only work on a single activity at a time.

* Example: `start = [1, 3, 0, 5, 8, 5]`, `end = [2, 4, 6, 7, 9, 9]`
* Output: `4` (The person can perform activities at indices 0, 1, 3, and 4).

**Core Idea:** You want to fit as many activities into your schedule as possible without any of their times overlapping.

---

## 1️⃣ Brute Force Approach (Recursion / Subsets)

The most naive way to solve this is to generate every single possible subset of activities using the "Pick / Not Pick" recursive pattern. We sort the activities by their start time, and then recursively try picking an activity (if it doesn't overlap with the previously picked activity) or skipping it, keeping track of the maximum count.

### JavaScript Code

```javascript
/**
 * Brute Force Approach (Recursion)
 * Explores all valid combinations of activities to find the maximum count.
 */
function maxActivitiesBruteForce(start, end) {
    let n = start.length;
    let activities = [];
    
    // Combine into an array of objects for easier sorting
    for (let i = 0; i < n; i++) {
        activities.push({ s: start[i], e: end[i] });
    }
    
    // Sort by start time so we can process them sequentially
    activities.sort((a, b) => a.s - b.s);
    
    function solve(index, lastEndTime) {
        // Base case: If we've checked all activities
        if (index === n) return 0;
        
        // Option 1: NOT PICK the current activity
        let notPick = solve(index + 1, lastEndTime);
        
        // Option 2: PICK the current activity (only if it doesn't overlap)
        let pick = 0;
        if (activities[index].s >= lastEndTime) {
            // Add 1 to count, and update the lastEndTime to this activity's end time
            pick = 1 + solve(index + 1, activities[index].e);
        }
        
        // Return the maximum of both choices
        return Math.max(notPick, pick);
    }
    
    // Start at index 0, with a lastEndTime of -1 (meaning no activities picked yet)
    return solve(0, -1);
}

// Example Usage
const start1 = [1, 3, 0, 5, 8, 5];
const end1   = [2, 4, 6, 7, 9, 9];
console.log("Brute Force Max Activities:", maxActivitiesBruteForce(start1, end1)); 
// Output: 4
```

### Complexity
* **Time Complexity:** $O(2^N)$ where $N$ is the number of activities. In the worst case, we branch into two decisions (pick or not pick) for every single activity. This will cause a Time Limit Exceeded (TLE) error for large arrays.
* **Space Complexity:** $O(N)$ auxiliary space strictly for the recursive call stack.

---

## 2️⃣ Optimal Approach (Greedy Algorithm)

We can completely eliminate the recursion by using a **Greedy** strategy. 

**The Mathematical Trick:** If we want to fit the *maximum* number of activities into a day, we should always prioritize the activity that **finishes the earliest**. Why? Because finishing an activity as early as possible leaves the maximum amount of remaining time to schedule future activities. 

Therefore, we **sort all activities by their END times**. Then, we simply iterate through them, picking an activity as long as its start time is greater than or equal to the end time of the last picked activity.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Greedy)
 * Sorts activities by end time and picks non-overlapping ones sequentially.
 */
function maxActivitiesOptimal(start, end) {
    let n = start.length;
    let activities = [];
    
    // Step 1: Combine into an array of objects
    for (let i = 0; i < n; i++) {
        activities.push({ start: start[i], end: end[i] });
    }
    
    // Step 2: Sort activities ascending strictly by END TIME
    activities.sort((a, b) => a.end - b.end);
    
    // Step 3: Greedily pick activities
    let maxCount = 1; // We always pick the first activity because it ends earliest
    let lastEndTime = activities[0].end;
    
    for (let i = 1; i < n; i++) {
        // If the current activity starts AFTER the last picked one ended
        if (activities[i].start >= lastEndTime) {
            maxCount++; // Pick it
            lastEndTime = activities[i].end; // Update the boundary
        }
    }
    
    return maxCount;
}

// Example Usage
const start2 = [1, 3, 0, 5, 8, 5];
const end2   = [2, 4, 6, 7, 9, 9];
console.log("Optimal Greedy Max Activities:", maxActivitiesOptimal(start2, end2));
// Output: 4
```

### Complexity
* **Time Complexity:** $O(N \log N)$ where $N$ is the number of activities. The time is entirely dominated by the sorting step. The subsequent greedy selection is a simple $O(N)$ linear pass.
* **Space Complexity:** $O(N)$ auxiliary space because we create an array of objects to link the start and end times together before sorting them.