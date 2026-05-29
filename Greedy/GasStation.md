# Gas Station (Circular Tour) in JavaScript ⛽

This repository contains the implementation of the **Gas Station** problem in JavaScript, following the Greedy Algorithms teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
There are $N$ gas stations along a circular route, where the amount of gas at the $i^{th}$ station is `gas[i]`. 
You have a car with an unlimited gas tank, and it costs `cost[i]` of gas to travel from the $i^{th}$ station to its next $(i + 1)^{th}$ station. You begin the journey with an empty tank at one of the gas stations.

Given two integer arrays `gas` and `cost`, return the **starting gas station's index** if you can travel around the circuit once in the clockwise direction, otherwise return `-1`. If there exists a solution, it is **guaranteed to be unique**.

* Example: `gas = [1, 2, 3, 4, 5]`, `cost = [3, 4, 5, 1, 2]`
* Output: `3` (Starting at index 3 yields enough fuel to complete the circular journey).

---

## 1️⃣ Brute Force Approach (Simulation)

The most naive way to solve this is to test every single gas station as a potential starting point. For every station $i$, we simulate the journey. We keep adding the gas from the current station and subtracting the cost to the next. If our tank ever drops below zero, this starting point is invalid. If we successfully loop back to $i$, we found our answer.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Simulates the entire circular journey starting from every single station.
 */
function canCompleteCircuitBruteForce(gas, cost) {
    let n = gas.length;
    
    // Try starting from every station 'i'
    for (let i = 0; i < n; i++) {
        let currentGas = 0;
        let isPossible = true;
        
        // Simulate the journey for 'n' steps
        for (let step = 0; step < n; step++) {
            // Calculate the actual index using modulo for circular behavior
            let currentStation = (i + step) % n;
            
            // Fuel up and drive
            currentGas += gas[currentStation] - cost[currentStation];
            
            // If tank drops below 0, we can't move forward
            if (currentGas < 0) {
                isPossible = false;
                break;
            }
        }
        
        // If we survived all 'n' steps, 'i' is the valid starting point
        if (isPossible) {
            return i;
        }
    }
    
    return -1;
}

// Example Usage
const gas1 = [1, 2, 3, 4, 5];
const cost1 = [3, 4, 5, 1, 2];
console.log("Brute Force Start Index:", canCompleteCircuitBruteForce(gas1, cost1)); 
// Output: 3
```

### Complexity
* **Time Complexity:** $O(N^2)$ where $N$ is the number of gas stations. In the worst case, we might travel almost the entire circle for every single starting point before failing. This will cause a Time Limit Exceeded (TLE) error for large arrays.
* **Space Complexity:** $O(1)$ auxiliary space.

---

## 2️⃣ Optimal Approach (Greedy Algorithm)

We can reduce this to $O(N)$ using a Greedy strategy based on two powerful mathematical observations:

1. **Global Check:** If the total sum of `gas` is strictly less than the total sum of `cost`, it is physically impossible to complete the circle, no matter where you start. If `totalGas >= totalCost`, a solution **must** exist.
2. **Local Check:** Suppose you start at station $A$ and successfully reach station $B$, but fail to reach station $C$ (because the tank drops below zero). This implies that **no station between $A$ and $B$ can be a valid starting point either.** Why? Because starting at $A$ gave you the maximum possible momentum to reach $C$. Starting anywhere after $A$ means you start with $0$ gas instead of whatever surplus you had carried from $A$.
   * **The Trick:** If you fail at $C$, the *only* logical next place to try starting from is $C + 1$.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Greedy Algorithm)
 * Uses a global counter to check overall feasibility, and a local counter 
 * to greedily advance the starting pointer when a failure occurs.
 */
function canCompleteCircuitOptimal(gas, cost) {
    let totalSurplus = 0; // Tracks overall gas vs cost
    let currentTank = 0;  // Tracks gas from the current starting point
    let startPosition = 0;
    
    for (let i = 0; i < gas.length; i++) {
        let netGasAtStation = gas[i] - cost[i];
        
        totalSurplus += netGasAtStation;
        currentTank += netGasAtStation;
        
        // If we ever run out of gas, this starting position (and everything before it) is invalid
        if (currentTank < 0) {
            // The next possible starting point is the very next station
            startPosition = i + 1;
            
            // Reset the current tank for the new starting point
            currentTank = 0;
        }
    }
    
    // If the total gas we picked up is at least the total cost, 
    // the 'startPosition' we found is mathematically guaranteed to work.
    return totalSurplus >= 0 ? startPosition : -1;
}

// Example Usage
const gas2 = [1, 2, 3, 4, 5];
const cost2 = [3, 4, 5, 1, 2];
console.log("Optimal Greedy Start Index:", canCompleteCircuitOptimal(gas2, cost2));
// Output: 3
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of gas stations. We iterate through the arrays exactly once.
* **Space Complexity:** $O(1)$ auxiliary space. We only use three integer variables (`totalSurplus`, `currentTank`, `startPosition`), making it perfectly memory optimized.