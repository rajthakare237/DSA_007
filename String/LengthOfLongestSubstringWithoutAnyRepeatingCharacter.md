# Length of Longest Substring Without Repeating Characters in JavaScript 🔠

This repository contains the implementation of the **Longest Substring Without Repeating Characters** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given a string `s`, find the length of the **longest substring** without repeating characters.

**Core Idea:** A substring is a contiguous sequence of characters within a string. We need to find the longest sequence where every character is unique. 

---

## 1️⃣ Brute Force Approach

The most basic way to solve this is to generate all possible substrings starting from each index and check if they contain unique characters. We use an outer loop to set the starting point, and an inner loop to expand the substring while keeping track of characters seen so far using a Hash Set.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Checks every possible substring and stops when a duplicate is found.
 */
function lengthOfLongestSubstringBruteForce(s) {
    let maxLength = 0;
    
    for (let i = 0; i < s.length; i++) {
        let seen = new Set();
        
        for (let j = i; j < s.length; j++) {
            // If we see a duplicate, break the inner loop
            if (seen.has(s[j])) {
                break;
            }
            
            seen.add(s[j]);
            maxLength = Math.max(maxLength, j - i + 1);
        }
    }
    
    return maxLength;
}

// Example Usage
const s1 = "abcabcbb";
console.log("Brute Force:", lengthOfLongestSubstringBruteForce(s1)); 
// Output: 3 ("abc")
```

### Complexity
* **Time Complexity:** $O(N^2)$ where $N$ is the length of the string. In the worst case, the inner loop runs for every character, which is too slow for very long strings.
* **Space Complexity:** $O(N)$ (or effectively $O(256)$ for ASCII characters) for the Hash Set used to store characters.

---

## 2️⃣ Better Approach (Sliding Window with Set)

We can optimize this using the **Sliding Window** technique with two pointers (`left` and `right`). We expand our window by moving the `right` pointer. If we encounter a duplicate, instead of starting entirely over, we shrink the window from the `left` until the duplicate character is removed from our Set.

### JavaScript Code

```javascript
/**
 * Better Approach (Sliding Window)
 * Uses two pointers and a Set to maintain a window of unique characters.
 */
function lengthOfLongestSubstringBetter(s) {
    let maxLength = 0;
    let left = 0;
    let seen = new Set();
    
    for (let right = 0; right < s.length; right++) {
        // If a duplicate is found, shrink the window from the left 
        // until the duplicate is completely removed
        while (seen.has(s[right])) {
            seen.delete(s[left]);
            left++;
        }
        
        // Add the current character and update max length
        seen.add(s[right]);
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}

// Example Usage
const s2 = "abcabcbb";
console.log("Better (Sliding Window):", lengthOfLongestSubstringBetter(s2));
// Output: 3 ("abc")
```

### Complexity
* **Time Complexity:** $O(2N)$. In the worst case, every character is visited twice: once by the `right` pointer to add it to the Set, and once by the `left` pointer to remove it.
* **Space Complexity:** $O(N)$ (or $O(256)$) for the Hash Set.

---

## 3️⃣ Optimal Approach (Sliding Window with Map)

While the better approach takes $O(2N)$ time, we can optimize it to strictly $O(N)$ time. 

**The Logic:** Instead of shrinking the window step-by-step using a `while` loop, we can store the **index** of the last occurrence of each character in a Hash Map. When the `right` pointer encounters a duplicate character, we can instantly jump the `left` pointer to `last_occurrence_index + 1` (as long as that index is within our current window bounds).

### JavaScript Code

```javascript
/**
 * Optimal Approach (Sliding Window with Index Mapping)
 * Stores the last seen index of characters to jump the 'left' pointer instantly.
 */
function lengthOfLongestSubstringOptimal(s) {
    let maxLength = 0;
    let left = 0;
    let map = new Map(); // Stores character -> last seen index
    
    for (let right = 0; right < s.length; right++) {
        let currentChar = s[right];
        
        // If the character was seen before AND its last index is inside our current window
        if (map.has(currentChar) && map.get(currentChar) >= left) {
            // Jump the left pointer directly past the previous duplicate
            left = map.get(currentChar) + 1;
        }
        
        // Update the last seen index of the character
        map.set(currentChar, right);
        
        // Calculate the length of the current valid window
        maxLength = Math.max(maxLength, right - left + 1);
    }
    
    return maxLength;
}

// Example Usage
const s3 = "abcabcbb";
console.log("Optimal (Index Map):", lengthOfLongestSubstringOptimal(s3));
// Output: 3 ("abc")
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the length of the string. Both pointers only move forward, and we never loop to shrink the window step-by-step.
* **Space Complexity:** $O(N)$ (or $O(256)$) for the Hash Map storing the indices.