# Valid Anagram in JavaScript 🔠🔄

This repository contains the implementation of the **Valid Anagram** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given two strings `s` and `t`, return `true` if `t` is an anagram of `s`, and `false` otherwise.

**Core Idea:** An anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once. Essentially, both strings must have the exact same characters with the exact same frequencies.

*(Edge Case Note: If the two strings have different lengths, they cannot be anagrams. We handle this immediately in all approaches).*

---

## 1️⃣ Brute Force Approach (Sorting)

The simplest way to check if two strings are anagrams is to sort both of them. If they are anagrams, their sorted versions will be perfectly identical. 

*(Note: In JavaScript, strings are immutable, so we must first split them into arrays, sort the arrays, and join them back into strings).*

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Sorts both strings and compares them for equality.
 */
function isAnagramBruteForce(s, t) {
    // If lengths are different, they cannot be anagrams
    if (s.length !== t.length) return false;
    
    // Split into array, sort, and join back to string
    let sortedS = s.split('').sort().join('');
    let sortedT = t.split('').sort().join('');
    
    // Compare the sorted strings
    return sortedS === sortedT;
}

// Example Usage
const s1 = "anagram";
const t1 = "nagaram";
console.log("Brute Force (Sorting):", isAnagramBruteForce(s1, t1)); 
// Output: true
```

### Complexity
* **Time Complexity:** $O(N \log N)$ where $N$ is the length of the strings. Sorting the arrays takes $O(N \log N)$ time.
* **Space Complexity:** $O(N)$ auxiliary space because we create new arrays when we use the `.split()` method.

---

## 2️⃣ Optimal Approach (Frequency Counter / Hashing)

We can optimize this to $O(N)$ time by using a frequency map or array. Since the problem usually constraints the strings to **lowercase English letters only**, we can use a fixed-size array of 26 elements instead of a Hash Map to save space and overhead.

**The Logic:** We iterate through both strings simultaneously. For every character in string `s`, we increment its count in the array. For every character in string `t`, we decrement its count. Finally, if they are anagrams, every single position in the array will have balanced out to `0`.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Frequency Array)
 * Uses a fixed array of size 26 to count character frequencies.
 */
function isAnagramOptimal(s, t) {
    // If lengths are different, they cannot be anagrams
    if (s.length !== t.length) return false;
    
    // Create an array of size 26 initialized with 0
    let count = new Array(26).fill(0);
    
    for (let i = 0; i < s.length; i++) {
        // Increment frequency for char in 's'
        // 'a'.charCodeAt(0) is 97, so we subtract 97 to map 'a'-'z' to index 0-25
        count[s.charCodeAt(i) - 97]++;
        
        // Decrement frequency for char in 't'
        count[t.charCodeAt(i) - 97]--;
    }
    
    // If they are anagrams, all counts should be perfectly 0
    for (let i = 0; i < 26; i++) {
        if (count[i] !== 0) {
            return false;
        }
    }
    
    return true;
}

// Example Usage
const s2 = "rat";
const t2 = "car";
console.log("Optimal (Frequency Array):", isAnagramOptimal(s2, t2));
// Output: false
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the length of the strings. We make one linear pass through the strings and one fixed pass through the 26-element array.
* **Space Complexity:** $O(1)$ auxiliary space. Even though we created an array, its size is strictly fixed at 26 regardless of how large the input strings are, making it constant space. *(If the problem included all Unicode characters, we would use a Map, which could take up to $O(N)$ space).*