# Fractional Knapsack Problem in JavaScript 🎒

This repository contains the implementation of the **Fractional Knapsack Problem** in JavaScript, following the Greedy Algorithms teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
You are given a set of $N$ items, each with a weight and a value, and a knapsack with a maximum weight capacity $W$. 

Your goal is to put items into the knapsack to maximize the total value. **You are allowed to break items into fractions** to maximize the total value (i.e., you can take a portion of an item if the whole item doesn't fit).

* Example: `W = 50`
* Items (Value, Weight): `[(60, 10), (100, 20), (120, 30)]`
* Output: `240` (Take the first item completely, the second item completely, and $2/3$ of the third item. Total value = $60 + 100 + (120 \times (20/30)) = 240$).

---

## 1️⃣ Optimal Approach (Greedy Algorithm)

Because we are allowed to take fractions of items, the most logical strategy is to always pick the item that gives us the **most bang for our buck**. We determine this by calculating the **Value-to-Weight Ratio** for every item.

**The Logic:**
1. Calculate the value/weight ratio for each item.
2. Sort the items in **descending order** based on this ratio.
3. Iterate through the sorted items:
   * If the knapsack has enough remaining capacity to hold the entire item, add its full weight and value.
   * If the knapsack cannot hold the entire item, calculate the remaining capacity, take exactly that fraction of the item, add its fractional value to your total, and break out of the loop (since the knapsack is now perfectly full).

### JavaScript Code

```javascript
/**
 * Class representing an Item in the knapsack
 */
class Item {
    constructor(value, weight) {
        this.value = value;
        this.weight = weight;
    }
}

/**
 * Optimal Approach (Greedy)
 * Sorts items by their Value/Weight ratio and fills the knapsack.
 */
function fractionalKnapsack(W, arr) {
    let n = arr.length;
    
    // Step 1: Sort items in descending order of their value/weight ratio
    arr.sort((a, b) => {
        let ratioA = a.value / a.weight;
        let ratioB = b.value / b.weight;
        return ratioB - ratioA;
    });

    let totalValue = 0.0;
    let currentWeight = 0;

    // Step 2: Greedily pick items
    for (let i = 0; i < n; i++) {
        // If adding the whole item won't exceed capacity
        if (currentWeight + arr[i].weight <= W) {
            currentWeight += arr[i].weight;
            totalValue += arr[i].value;
        } 
        // If we can only take a fraction of the current item
        else {
            let remainingWeight = W - currentWeight;
            // Add the value of the fraction we can take
            totalValue += (arr[i].value / arr[i].weight) * remainingWeight;
            
            // The knapsack is now completely full, so we stop
            break;
        }
    }

    return totalValue;
}

// ---- Example Usage ----
let W = 50;
let arr = [
    new Item(60, 10),
    new Item(100, 20),
    new Item(120, 30)
];

console.log("Maximum Value in Knapsack:", fractionalKnapsack(W, arr));
// Output: 240
```

### Complexity
* **Time Complexity:** $O(N \log N)$ where $N$ is the number of items. The time is entirely dominated by the sorting step. Iterating through the array takes an additional $O(N)$ time.
* **Space Complexity:** $O(1)$ auxiliary space. We modify the input array by sorting it directly and only use a few variables (`totalValue`, `currentWeight`) to keep track of the knapsack's state.