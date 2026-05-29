# Find Middle Element in a Linked List in JavaScript 🎯

This repository contains the implementation of the **Find Middle Element in a Linked List** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given the `head` of a singly linked list, return the **middle node** of the linked list. 

**Rule:** If there are two middle nodes (which happens when the number of nodes is even), return the **second** middle node.
* Odd Length: `[1, 2, 3, 4, 5]` ➔ Middle is `3`
* Even Length: `[1, 2, 3, 4, 5, 6]` ➔ Middle is `4`

### Linked List Node Definition
For context, here is the standard implementation of a Linked List node used in the solutions below:
```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}
```

---

## 1️⃣ Brute Force Approach (Two Passes)

The most intuitive way to find the middle is to simply figure out how long the linked list is. We traverse the list once to count the total number of nodes ($N$). Once we have the length, we divide it by 2 to find the middle index. We then traverse the list a second time, stopping exactly at that middle index.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Traverses once to count length, then a second time to reach the middle.
 */
function middleNodeBruteForce(head) {
    let length = 0;
    let temp = head;
    
    // Step 1: Traverse to count the total length of the list
    while (temp !== null) {
        length++;
        temp = temp.next;
    }
    
    // Step 2: Calculate the exact middle position (0-indexed)
    let midIndex = Math.floor(length / 2);
    
    // Step 3: Traverse again to reach the middle node
    temp = head;
    for (let i = 0; i < midIndex; i++) {
        temp = temp.next;
    }
    
    return temp; // This is the middle node
}

// Example Usage setup is at the bottom of the document
```

### Complexity
* **Time Complexity:** $O(N + N/2)$ where $N$ is the number of nodes. We traverse the entire list once, and then traverse half of it again. Overall, this simplifies to $O(N)$ time.
* **Space Complexity:** $O(1)$ auxiliary space. We only use a few variables (`length`, `midIndex`, `temp`).

---

## 2️⃣ Optimal Approach (Tortoise and Hare / One Pass)

We can achieve a strict one-pass solution by using two pointers: `slow` (the Tortoise) and `fast` (the Hare). 

**The Logic:** Both pointers start at the `head`. 
* The `slow` pointer moves forward exactly **one** step at a time.
* The `fast` pointer moves forward exactly **two** steps at a time.
* Because the `fast` pointer is moving twice as fast, by the time the `fast` pointer reaches the very end of the list, the `slow` pointer will naturally be stuck exactly halfway through—right at the middle node!

### JavaScript Code

```javascript
/**
 * Optimal Approach (Fast & Slow Pointers)
 * Moves a fast pointer 2 steps and a slow pointer 1 step. 
 * When fast reaches the end, slow is at the middle.
 */
function middleNodeOptimal(head) {
    let slow = head;
    let fast = head;
    
    // Continue until 'fast' reaches the last node or steps out of bounds
    while (fast !== null && fast.next !== null) {
        slow = slow.next;        // Slow moves 1 step
        fast = fast.next.next;   // Fast moves 2 steps
    }
    
    // 'slow' is now pointing exactly to the middle node
    return slow; 
}

// ---- Example Usage ----

// Helper to easily print the remaining list starting from the middle
function printListFromNode(node) {
    let result = [];
    while (node !== null) {
        result.push(node.val);
        node = node.next;
    }
    console.log(result.join(" -> "));
}

// Example 1: Odd length list (1 -> 2 -> 3 -> 4 -> 5)
let headOdd = new ListNode(1, new ListNode(2, new ListNode(3, new ListNode(4, new ListNode(5)))));
console.log("Odd Length List Middle Node value:", middleNodeOptimal(headOdd).val);
// Output: 3

// Example 2: Even length list (1 -> 2 -> 3 -> 4 -> 5 -> 6)
let headEven = new ListNode(1, new ListNode(2, new ListNode(3, new ListNode(4, new ListNode(5, new ListNode(6))))));
console.log("Even Length List Middle Node value:", middleNodeOptimal(headEven).val);
// Output: 4
```

### Complexity
* **Time Complexity:** $O(N/2)$ which simplifies to $O(N)$. The `fast` pointer skips nodes, allowing the algorithm to finish in roughly half the steps of a full traversal.
* **Space Complexity:** $O(1)$ auxiliary space. We only allocate memory for two pointers (`slow` and `fast`), making it highly memory efficient.