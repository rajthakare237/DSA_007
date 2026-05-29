# Remove Nth Node From End of List in JavaScript 🗑️

This repository contains the implementation of the **Remove Nth Node From End of List** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given the `head` of a linked list, remove the $N^{th}$ node from the end of the list and return its head.

**Core Idea:** Linked lists do not have backward traversal (unless doubly linked) or an index property. Finding the end requires traversing it. The challenge is identifying which node is exactly $N$ steps from the end while traversing forward.

### Linked List Node Definition
For context, here is the standard implementation of a Linked List node used in the solutions below:
```javascript
class ListNode {
    constructor(val = 0, next = null) {
        this.val = val;
        this.next = next;
    }
}

// Helper function to print the list (for testing)
function printList(head) {
    let result = [];
    while (head !== null) {
        result.push(head.val);
        head = head.next;
    }
    console.log(result.join(" -> "));
}
```

---

## 1️⃣ Brute Force Approach (Two Passes)

The most intuitive way to solve this is to first figure out exactly how long the linked list is. We traverse the list once to count the total nodes ($L$). Once we know the length, the $N^{th}$ node from the end is simply the $(L - N + 1)^{th}$ node from the beginning. We do a second traversal to reach the node *just before* it, and change its `next` pointer to skip the target node.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Traverses once to count length, then a second time to find the target.
 */
function removeNthFromEndBruteForce(head, n) {
    let length = 0;
    let temp = head;
    
    // Step 1: Count the total number of nodes
    while (temp !== null) {
        length++;
        temp = temp.next;
    }
    
    // Edge Case: If n equals the length, we are asked to remove the head node
    if (n === length) {
        return head.next;
    }
    
    // Step 2: Find the node just BEFORE the target node (Length - N)
    let stepsToTake = length - n;
    temp = head;
    for (let i = 1; i < stepsToTake; i++) {
        temp = temp.next;
    }
    
    // Step 3: Delete the target node by skipping it
    temp.next = temp.next.next;
    
    return head;
}

// Example Usage setup is at the bottom of the document
```

### Complexity
* **Time Complexity:** $O(L) + O(L - N)$ where $L$ is the total length of the list. In the worst case (where $N=1$), we traverse the list almost twice, making it $O(2L)$.
* **Space Complexity:** $O(1)$ auxiliary space, as we are only using a few variable pointers.

---

## 2️⃣ Optimal Approach (Fast & Slow Pointers / One Pass)

We can do this in a single pass by using two pointers: `fast` and `slow`. 

**The Logic:** 1. We move the `fast` pointer $N$ steps ahead of the `slow` pointer. This creates a "gap" of $N$ nodes between them.
2. We then move both pointers forward one step at a time.
3. Because the `fast` pointer has a head start of $N$ nodes, when the `fast` pointer reaches the very last node, the `slow` pointer will naturally be exactly $N$ nodes behind it—which is right before the node we want to delete!

### JavaScript Code

```javascript
/**
 * Optimal Approach (Fast & Slow Pointers)
 * Maintains a gap of 'N' between two pointers to find the target in one pass.
 */
function removeNthFromEndOptimal(head, n) {
    let fast = head;
    let slow = head;
    
    // Step 1: Move the 'fast' pointer N steps ahead
    for (let i = 0; i < n; i++) {
        fast = fast.next;
    }
    
    // Edge Case: If 'fast' becomes null after moving N steps, 
    // it means N is exactly equal to the length of the list.
    // Therefore, the node to delete is the head.
    if (fast === null) {
        return head.next;
    }
    
    // Step 2: Move both pointers until 'fast' reaches the last node
    while (fast.next !== null) {
        fast = fast.next;
        slow = slow.next;
    }
    
    // Step 3: 'slow' is now positioned exactly before the node to delete.
    // Skip the target node.
    slow.next = slow.next.next;
    
    return head;
}

// ---- Example Usage ----
// Build list: 1 -> 2 -> 3 -> 4 -> 5
let head1 = new ListNode(1, new ListNode(2, new ListNode(3, new ListNode(4, new ListNode(5)))));

console.log("Original List:");
printList(head1);

// Remove the 2nd node from the end (which is '4')
let newHead = removeNthFromEndOptimal(head1, 2);

console.log("List after removing 2nd node from end:");
printList(newHead);
// Output: 1 -> 2 -> 3 -> 5
```

### Complexity
* **Time Complexity:** $O(L)$ where $L$ is the length of the linked list. The `fast` pointer traverses the list exactly once, making it a true single-pass solution.
* **Space Complexity:** $O(1)$ auxiliary space. We only use two pointers (`fast` and `slow`), meaning memory does not scale with the size of the list.