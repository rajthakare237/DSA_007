# Reverse a Linked List in JavaScript 🔄

This repository contains the implementation of the **Reverse a Linked List** problem in JavaScript. The progression follows standard algorithmic teaching patterns, moving from a data-swapping brute-force method to optimal pointer-manipulation methods.

---

## 📖 What is the Problem?
Given the `head` of a singly linked list, reverse the list, and return the reversed list.

**Core Idea:** Instead of just changing the values inside the nodes, the most optimal way to reverse a linked list is to physically reverse the direction of the `next` pointers.

### Linked List Node Definition
For context, here is the standard implementation of a Linked List node in JavaScript used in the solutions below:
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

## 1️⃣ Brute Force Approach (Using a Stack/Array)

The most intuitive way to reverse the elements is to do a two-pass traversal. In the first pass, we traverse the list and push all the values into a Stack (or an Array). In the second pass, we traverse the list again from the beginning and overwrite the node values with the popped values from the stack.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Uses an external array/stack to store values and overwrites the linked list.
 */
function reverseListBruteForce(head) {
    let stack = [];
    let temp = head;
    
    // Step 1: Push all values into the stack
    while (temp !== null) {
        stack.push(temp.val);
        temp = temp.next;
    }
    
    // Step 2: Pop values to overwrite original list data
    temp = head;
    while (temp !== null) {
        temp.val = stack.pop();
        temp = temp.next;
    }
    
    return head;
}

// Example Usage
let head1 = new ListNode(1, new ListNode(2, new ListNode(3, new ListNode(4))));
console.log("Original List:");
printList(head1);

reverseListBruteForce(head1);
console.log("Brute Force Reversed:");
printList(head1);
```

### Complexity
* **Time Complexity:** $O(2N)$ where $N$ is the number of nodes. We traverse the list completely twice.
* **Space Complexity:** $O(N)$ auxiliary space. We use an external array/stack to store the $N$ values.

---

## 2️⃣ Optimal Approach (Iterative / Three Pointers)

To optimize the space complexity to $O(1)$, we must reverse the list **in-place**. We do this by reversing the actual `next` pointers of the nodes. We need three pointers: `prev`, `current`, and `next` to keep track of our positions without losing the rest of the list.

### JavaScript Code

```javascript
/**
 * Optimal Iterative Approach
 * Reverses the 'next' pointers in a single pass using three pointers.
 */
function reverseListIterative(head) {
    let prev = null;
    let current = head;
    let next = null;
    
    while (current !== null) {
        // Step 1: Save the next node so we don't lose the rest of the list
        next = current.next; 
        
        // Step 2: Reverse the link
        current.next = prev; 
        
        // Step 3: Move the 'prev' and 'current' pointers one step forward
        prev = current;
        current = next;
    }
    
    // 'prev' will be standing at the last node, which is our new head
    return prev; 
}

// Example Usage
let head2 = new ListNode(1, new ListNode(2, new ListNode(3, new ListNode(4))));
let newHeadIterative = reverseListIterative(head2);
console.log("Iterative Optimal Reversed:");
printList(newHeadIterative);
```

### Complexity
* **Time Complexity:** $O(N)$. We make a single pass through the linked list.
* **Space Complexity:** $O(1)$ auxiliary space, as we are only using three pointers regardless of the list size.

---

## 3️⃣ Optimal Approach (Recursive)

This problem is frequently tested using recursion. The recursive approach travels all the way to the end of the list. Once it hits the last node, it returns that node as the `newHead`. Then, as the recursive functions resolve (move back up the call stack), we reverse the links step-by-step.

### JavaScript Code

```javascript
/**
 * Optimal Recursive Approach
 * Reverses the list by utilizing the recursive call stack.
 */
function reverseListRecursive(head) {
    // Base Case: If list is empty or we reached the last node
    if (head === null || head.next === null) {
        return head; // This last node becomes the new head
    }
    
    // Step 1: Recursively reach the end of the list
    let newHead = reverseListRecursive(head.next);
    
    // Step 2: Reverse the link between the next node and the current node
    let front = head.next;
    front.next = head;
    
    // Step 3: Break the original forward link to avoid cycles
    head.next = null;
    
    // Pass the new head back up the recursive chain
    return newHead; 
}

// Example Usage
let head3 = new ListNode(1, new ListNode(2, new ListNode(3, new ListNode(4))));
let newHeadRecursive = reverseListRecursive(head3);
console.log("Recursive Optimal Reversed:");
printList(newHeadRecursive);
```

### Complexity
* **Time Complexity:** $O(N)$. Every node is visited once during the recursive calls.
* **Space Complexity:** $O(N)$ auxiliary space. Although we aren't creating arrays, the recursive call stack will stack up $N$ frames deep before it resolves.