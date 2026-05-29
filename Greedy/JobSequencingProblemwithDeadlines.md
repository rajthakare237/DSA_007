# Job Sequencing Problem with Deadlines in JavaScript 💼

This repository contains the implementation of the **Job Sequencing Problem** in JavaScript, following the Greedy Algorithms teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
You are given a set of $N$ jobs where each job comes with a `deadline` and a `profit`. 
* Every job takes exactly **1 unit of time** to complete.
* A job earns its profit only if it is completed on or before its deadline.
* You can only perform one job at a time.

**Your goal:** Maximize the total profit and find the number of jobs done.

* Example:
  * Jobs (ID, Deadline, Profit): `[(1, 4, 20), (2, 1, 10), (3, 1, 40), (4, 1, 30)]`
  * Output: `2 jobs done, Total Profit: 60` (Job 3 is done at time 1, Job 1 is done at time 4).

---

## 1️⃣ Brute Force Approach (Permutations)

The most naive way to solve this is to generate all possible sequences (permutations) of the jobs. For each sequence, you would check if the jobs can be completed within their respective deadlines and calculate the profit. You would then return the maximum profit found across all valid sequences.

### Complexity
* **Time Complexity:** $O(N!)$ where $N$ is the number of jobs. Generating all permutations is highly inefficient and will result in a Time Limit Exceeded (TLE) error for even slightly larger arrays.
* **Space Complexity:** $O(N)$ for the recursion stack used to generate permutations.

*(Note: Because $O(N!)$ is entirely impractical, code for this approach is rarely written in interviews. The discussion immediately shifts to the Greedy approach).*

---

## 2️⃣ Optimal Approach (Greedy Algorithm + Slot Array)

To maximize our profit, we should obviously prioritize the jobs that yield the highest profit. However, to leave room for other jobs, we should delay doing a job for as long as possible (i.e., we should perform the job **as close to its deadline as possible**).

**The Logic:**
1. **Sort the jobs** in descending order based on their profit.
2. Find the maximum deadline among all jobs.
3. Create a `slots` array of size `max_deadline + 1` initialized to `-1` (indicating empty slots).
4. Iterate through the sorted jobs:
   * For each job, look at its deadline. 
   * Try to place the job in the slot exactly at its deadline.
   * If that slot is taken, search backwards (decrementing the slot index) until you find an empty slot.
   * If you reach slot 0 without finding an empty space, the job is dropped.

### JavaScript Code

```javascript
/**
 * Class representing a Job
 */
class Job {
    constructor(id, deadline, profit) {
        this.id = id;
        this.deadline = deadline;
        this.profit = profit;
    }
}

/**
 * Optimal Approach (Greedy Algorithm)
 * Sorts by highest profit, then schedules the job as late as possible.
 */
function jobScheduling(jobs) {
    let n = jobs.length;

    // Step 1: Sort jobs in descending order of profit
    jobs.sort((a, b) => b.profit - a.profit);

    // Step 2: Find the maximum deadline to size our slots array
    let maxDeadline = 0;
    for (let i = 0; i < n; i++) {
        if (jobs[i].deadline > maxDeadline) {
            maxDeadline = jobs[i].deadline;
        }
    }

    // Step 3: Create an array to keep track of free time slots
    // We use maxDeadline + 1 so we can use 1-based indexing for days
    let slots = new Array(maxDeadline + 1).fill(-1);

    let countJobs = 0;
    let maxProfit = 0;

    // Step 4: Iterate through all jobs and place them
    for (let i = 0; i < n; i++) {
        // Try to place the job at its deadline, or as close to it as possible (searching backwards)
        for (let j = jobs[i].deadline; j > 0; j--) {
            // If the slot is empty, schedule the job here
            if (slots[j] === -1) {
                slots[j] = jobs[i].id; // Mark slot as filled with Job ID
                countJobs++;           // Increment completed jobs count
                maxProfit += jobs[i].profit; // Add to total profit
                
                break; // Job is scheduled, move to the next job
            }
        }
    }

    return [countJobs, maxProfit];
}

// ---- Example Usage ----
let jobs = [
    new Job(1, 4, 20),
    new Job(2, 1, 10),
    new Job(3, 1, 40),
    new Job(4, 1, 30)
];

let result = jobScheduling(jobs);
console.log(`Jobs Scheduled: ${result[0]}`);
console.log(`Total Maximum Profit: ${result[1]}`);
// Output: 
// Jobs Scheduled: 2
// Total Maximum Profit: 60
```

### Complexity
* **Time Complexity:** $O(N \log N) + O(N \times M)$ where $N$ is the number of jobs and $M$ is the maximum deadline. Sorting takes $O(N \log N)$. For each of the $N$ jobs, we might scan up to $M$ slots backwards in the worst case. 
* **Space Complexity:** $O(M)$ auxiliary space. We create a `slots` array of size $M + 1$ to track the scheduled days.