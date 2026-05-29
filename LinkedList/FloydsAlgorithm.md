# Linked List Cycle Start Node (Floyd’s Algorithm) in JavaScript 🐢🐇

This repository contains the implementation of the **Linked List Cycle Detection and Start Node** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given the `head` of a singly linked list, determine if the linked list has a cycle in it. If a cycle exists, return the **node where the cycle begins**. If there is no cycle, return `null`.

**Core Idea:** A cycle occurs if a node's `next` pointer points back to a previous node in the list. 

### Linked List Node Definition
For context, here is the standard implementation of a Linked List node used in the solutions below:
```javascript
class ListNode {
    constructor(val) {
        this.val = val;
        this.next = null;
    }
}
```

---

## 1️⃣ Brute Force Approach (Hashing)

The most intuitive way to find the starting point of a cycle is to remember every node we visit. As we traverse the linked list, we store the nodes (the actual node references, not just their values) in a Hash Set. The moment we encounter a node that is *already* in our Hash Set, we have found the starting node of the cycle.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Uses a Hash Set to track visited nodes.
 */
function findCycleStartBruteForce(head) {
    let visited = new Set();
    let current = head;
    
    while (current !== null) {
        // If the node is already in the set, it's the start of the cycle
        if (visited.has(current)) {
            return current;
        }
        
        // Otherwise, mark this node as visited and move forward
        visited.add(current);
        current = current.next;
    }
    
    return null; // Reached the end of the list, no cycle
}

// Example Usage setup is at the bottom of the document
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of nodes in the list. We visit each node at most once. Set insertions and lookups take $O(1)$ time on average.
* **Space Complexity:** $O(N)$ auxiliary space. In the worst-case scenario (a cycle at the very end, or no cycle at all), we will store all $N$ nodes in the Hash Set.

---

## 2️⃣ Optimal Approach (Floyd’s Tortoise & Hare)

To optimize the space complexity to $O(1)$, we use Floyd’s Cycle Detection algorithm. 

**The Logic:**
1. **Detect the Cycle:** Use two pointers, `slow` (moves 1 step) and `fast` (moves 2 steps). If there is a cycle, they are guaranteed to meet. If `fast` reaches `null`, there is no cycle.
2. **Find the Start:** Once they meet, the distance from the `head` to the cycle start equals the distance from their `meeting point` to the cycle start. We move one pointer back to the `head` and keep the other at the meeting point. If we advance both by 1 step at a time, they will collide exactly at the cycle's starting node.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Floyd's Algorithm)
 * Uses slow and fast pointers to detect the cycle, then finds the starting node.
 */
function findCycleStartOptimal(head) {
    let slow = head;
    let fast = head;
    let hasCycle = false;

    // Step 1: Detect cycle (Tortoise and Hare)
    while (fast !== null && fast.next !== null) {
        slow = slow.next;           // move 1 step
        fast = fast.next.next;      // move 2 steps
        
        if (slow === fast) {
            hasCycle = true;        // they met -> cycle detected
            break;
        }
    }

    if (!hasCycle) return null; // no cycle exists

    // Step 2: Find cycle start node
    let p1 = head;
    let p2 = slow; // slow is currently at the meeting point
    
    // Advance both one step at a time; they will meet at the cycle start
    while (p1 !== p2) {
        p1 = p1.next;
        p2 = p2.next;
    }
    
    return p1; // start of the cycle
}

// ---- Example Usage ----
// Build list: 1 -> 2 -> 3 -> 4 -> 5
const nodes = [1, 2, 3, 4, 5].map(n => new ListNode(n));
for (let i = 0; i < nodes.length - 1; i++) {
    nodes[i].next = nodes[i + 1];
}

// Create cycle: 5 -> 3 (cycle starts at node with val=3)
nodes[nodes.length - 1].next = nodes[2]; 

const cycleStartNode = findCycleStartOptimal(nodes[0]);

if (cycleStartNode) {
    console.log("Cycle detected. Starts at node with value:", cycleStartNode.val);
} else {
    console.log("No cycle detected.");
}
// Output: Cycle detected. Starts at node with value: 3
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the number of nodes. The `fast` pointer traverses the list linearly to detect the cycle, and the second phase takes at most $N$ steps to find the start.
* **Space Complexity:** $O(1)$ auxiliary space. We only use a few pointers (`slow`, `fast`, `p1`, `p2`) regardless of the size of the linked list.