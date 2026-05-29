# Check for Balanced Parentheses in JavaScript ⚖️

This repository contains the implementation of the **Balanced Parentheses** problem in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:
1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

---

## 1️⃣ Brute Force Approach (String Manipulation)

The most naive way to solve this without using a data structure is to repeatedly find and remove valid, adjacent pairs of parentheses (`"()"`, `"{}"`, `"[]"`) from the string. If the string is perfectly balanced, we will eventually reduce it to an empty string. 

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Continually replaces matched pairs with an empty string.
 */
function isValidBruteForce(s) {
    let prevLength = -1;
    
    // Keep looping as long as the string length is shrinking
    while (s.length !== prevLength) {
        prevLength = s.length;
        
        // Remove adjacent balanced pairs
        s = s.replace('()', '');
        s = s.replace('{}', '');
        s = s.replace('[]', '');
    }
    
    // If the string is empty, it was perfectly balanced
    return s.length === 0;
}

// Example Usage
const s1 = "{[()]}";
console.log("Brute Force:", isValidBruteForce(s1)); 
// Output: true
```

### Complexity
* **Time Complexity:** $O(N^2)$ in the worst-case scenario (e.g., `"((((({}))))))"`). The `replace` method scans the string, and we might have to scan it $N/2$ times.
* **Space Complexity:** $O(N)$ because strings are immutable in JavaScript, so `replace` creates a new string in memory during each iteration.

---

## 2️⃣ Optimal Approach (Using a Stack)

The standard and most optimal way to solve this problem is by using a **Stack** data structure (which operates on a Last-In-First-Out / LIFO principle).

**The Logic:** 1. As we iterate through the string, if we see an **opening** bracket, we push it onto the stack.
2. If we see a **closing** bracket, we check the top of our stack. 
3. If the top of the stack has the corresponding opening bracket, we pop it off (they cancel each other out). If it doesn't match, or if the stack is empty, the string is invalid.
4. At the end of the string, if the stack is completely empty, it means every bracket found a matching partner.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Stack)
 * Uses an array as a stack to match opening brackets with closing ones.
 */
function isValidOptimal(s) {
    let stack = [];
    
    for (let i = 0; i < s.length; i++) {
        let char = s[i];
        
        // If it's an opening bracket, push it to the stack
        if (char === '(' || char === '{' || char === '[') {
            stack.push(char);
        } 
        // If it's a closing bracket
        else {
            // If the stack is empty, there is no matching opening bracket
            if (stack.length === 0) return false;
            
            // Pop the last added opening bracket
            let top = stack.pop();
            
            // Check if it matches the current closing bracket
            if (char === ')' && top !== '(') return false;
            if (char === '}' && top !== '{') return false;
            if (char === ']' && top !== '[') return false;
        }
    }
    
    // If the stack is empty, all brackets were successfully matched
    return stack.length === 0;
}

// Example Usage
const s2 = "()[{}()]";
console.log("Optimal (Stack):", isValidOptimal(s2));
// Output: true

const s3 = "[(])";
console.log("Optimal (Stack) Invalid Case:", isValidOptimal(s3));
// Output: false
```

### Complexity
* **Time Complexity:** $O(N)$ where $N$ is the length of the string. We iterate through the string exactly once, and `push`/`pop` array operations in JavaScript take $O(1)$ time.
* **Space Complexity:** $O(N)$ auxiliary space. In the worst-case scenario (e.g., `"(((((((("`), we will push all $N$ characters onto the stack.