# Merge Two Sorted Linked Lists in JavaScript 🔗

This repository contains the implementation of the **Merge Two Sorted Linked Lists** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given the heads of two **sorted** linked lists `list1` and `list2`, merge the two lists into one single sorted list. The list should be made by splicing together the nodes of the first two lists.

Return the head of the merged linked list.

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

## 1️⃣ Brute Force Approach (Using an Array)

The most naive way to solve this is to completely ignore the fact that the two lists are already sorted. We simply traverse both lists, push all their values into an array, sort the array, and then create a brand new linked list from the sorted array.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Extracts all values into an array, sorts them, and creates a new linked list.
 */
function mergeTwoListsBruteForce(list1, list2) {
    let arr = [];
    
    // Step 1: Extract values from the first list
    let temp1 = list1;
    while (temp1 !== null) {
        arr.push(temp1.val);
        temp1 = temp1.next;
    }
    
    // Step 2: Extract values from the second list
    let temp2 = list2;
    while (temp2 !== null) {
        arr.push(temp2.val);
        temp2 = temp2.next;
    }
    
    // Step 3: Sort the array in ascending order
    arr.sort((a, b) => a - b);
    
    // Step 4: Create a completely new linked list
    let dummy = new ListNode(-1);
    let current = dummy;
    
    for (let i = 0; i < arr.length; i++) {
        current.next = new ListNode(arr[i]);
        current = current.next;
    }
    
    return dummy.next;
}

// Example Usage setup is at the bottom of the document
```

### Complexity
* **Time Complexity:** $O((N_1 + N_2) \log(N_1 + N_2))$ where $N_1$ and $N_2$ are the lengths of the two lists. The time is heavily dominated by the array sorting step.
* **Space Complexity:** $O(N_1 + N_2)$ auxiliary space. We create an array to hold all values, plus a completely new linked list.

---

## 2️⃣ Optimal Approach (Two Pointers + Dummy Node)

Since both linked lists are already sorted, we do not need extra space. We can just use two pointers to simultaneously traverse `list1` and `list2`, compare their current values, and rewire the `next` pointers to point to the smaller value. 

We use a **Dummy Node** to easily handle the edge case of initializing the head of our new merged list.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Two Pointers In-Place)
 * Rewires the existing nodes without allocating new memory for a new list.
 */
function mergeTwoListsOptimal(list1, list2) {
    // Create a dummy node to act as the starting point
    let dummy = new ListNode(-1);
    let current = dummy;
    
    let t1 = list1;
    let t2 = list2;
    
    // Step 1: Traverse both lists while neither is empty
    while (t1 !== null && t2 !== null) {
        if (t1.val <= t2.val) {
            current.next = t1; // Link to the smaller node
            t1 = t1.next;      // Move list1 pointer forward
        } else {
            current.next = t2; // Link to the smaller node
            t2 = t2.next;      // Move list2 pointer forward
        }
        // Move our merged list pointer forward
        current = current.next; 
    }
    
    // Step 2: If any elements are remaining in either list, simply attach them
    // (Because the lists are already sorted, the remainder is guaranteed to be in order)
    if (t1 !== null) {
        current.next = t1;
    } else if (t2 !== null) {
        current.next = t2;
    }
    
    // The merged list starts immediately after the dummy node
    return dummy.next;
}

// ---- Example Usage ----
// List 1: 1 -> 2 -> 4
let l1 = new ListNode(1, new ListNode(2, new ListNode(4)));

// List 2: 1 -> 3 -> 4
let l2 = new ListNode(1, new ListNode(3, new ListNode(4)));

console.log("Optimal Merged List:");
let mergedHead = mergeTwoListsOptimal(l1, l2);
printList(mergedHead);
// Output: 1 -> 1 -> 2 -> 3 -> 4 -> 4
```

### Complexity
* **Time Complexity:** $O(N_1 + N_2)$ where $N_1$ and $N_2$ are the lengths of the two lists. In the worst case, we traverse both lists entirely exactly once.
* **Space Complexity:** $O(1)$ auxiliary space. We are simply rewiring the existing `next` pointers and only using a few variable pointers (`dummy`, `current`, `t1`, `t2`), meaning no new scaling memory is required.