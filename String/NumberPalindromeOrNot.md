# Check if a Number is a Palindrome in JavaScript 🪞

This repository contains the implementation to **Check if a Number is a Palindrome** in JavaScript, following the teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
Given an integer `n`, return `true` if `n` is a palindrome, and `false` otherwise.

An integer is a **palindrome** when it reads the same forward and backward. For example, `121` is a palindrome while `123` is not. 

*(Edge Case Note: Negative numbers are generally not considered palindromes because the minus sign comes first. For example, `-121` reversed is `121-`, which is not equal to `-121`.)*

---

## 1️⃣ Brute Force Approach (String Conversion)

The easiest way to solve this is to convert the integer into a string. Once it is a string, we can split it into an array of characters, reverse the array, join it back into a string, and check if it matches the original string.

### JavaScript Code

```javascript
/**
 * Brute Force Approach
 * Converts the number to a string, reverses it, and compares.
 */
function isPalindromeString(n) {
    // Negative numbers are not palindromes
    if (n < 0) return false;
    
    let originalStr = n.toString();
    
    // Split into array, reverse, and join back to string
    let reversedStr = originalStr.split('').reverse().join('');
    
    return originalStr === reversedStr;
}

// Example Usage
const num1 = 121;
console.log("Brute Force (String):", isPalindromeString(num1)); 
// Output: true

const num2 = -121;
console.log("Brute Force (String) Negative:", isPalindromeString(num2)); 
// Output: false
```

### Complexity
* **Time Complexity:** $O(d)$ where $d$ is the number of digits in the number (which is $\log_{10} N$). Converting to a string, splitting, reversing, and joining all take linear time relative to the number of digits.
* **Space Complexity:** $O(d)$ auxiliary space because we create a new string and a new array in memory to hold the reversed characters.

---

## 2️⃣ Optimal Approach (Mathematical Reversal)

To optimize the space complexity, we can solve this entirely using math. We can extract the digits of the number one by one from the end (using the modulo operator `%`) and build a completely new reversed number. Finally, we compare this new reversed number with the original number.

### JavaScript Code

```javascript
/**
 * Optimal Approach (Math)
 * Mathematically reverses the number without converting it to a string.
 */
function isPalindromeOptimal(n) {
    // Negative numbers are not palindromes
    if (n < 0) return false;
    
    let original = n;
    let reversed = 0;
    
    // Mathematically build the reversed number
    while (n > 0) {
        let digit = n % 10;            // Extract the last digit
        reversed = (reversed * 10) + digit; // Append digit to the reversed number
        n = Math.floor(n / 10);        // Remove the last digit from n
    }
    
    // Check if the mathematically reversed number matches the original
    return original === reversed;
}

// Example Usage
const num3 = 121;
console.log("Optimal (Math):", isPalindromeOptimal(num3));
// Output: true

const num4 = 123;
console.log("Optimal (Math) Non-Palindrome:", isPalindromeOptimal(num4));
// Output: false
```

### Complexity
* **Time Complexity:** $O(\log_{10} N)$ where $N$ is the number itself. The `while` loop runs as many times as there are digits in the number.
* **Space Complexity:** $O(1)$ auxiliary space. We do not convert the number to a string or array, we only use a couple of variables to store integers, making this the most memory-efficient approach.