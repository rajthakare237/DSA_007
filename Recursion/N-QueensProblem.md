# The N-Queens Problem in JavaScript 👑

This repository contains the implementation of the **N-Queens Problem** in JavaScript, following the Backtracking teaching pattern from [takeuforward.org](https://takeuforward.org).

---

## 📖 What is the Problem?
The **$N$-Queens puzzle** is the problem of placing $N$ chess queens on an $N \times N$ chessboard so that no two queens threaten each other. 

Thus, a solution requires that no two queens share the same **row, column, or diagonal**. Given an integer `n`, return all distinct solutions to the $n$-queens puzzle. You may return the answer in any order.

* Example: `n = 4`
* Output: 
```text
[
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],

 ["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
```

---

## 1️⃣ Approach 1: Standard Backtracking (Iterative Safety Check)

We place queens column by column. For every column, we try placing a queen in every row (from $0$ to $N-1$). Before placing the queen, we check if the cell is "safe".

**The Logic:**
Because we are filling the board from left to right (column by column), we don't need to check the right side of the board. We only need to check three directions for attacking queens:
1. **Left Row:** Is there a queen in the same row to the left?
2. **Upper Left Diagonal:** Is there a queen diagonally up-left?
3. **Lower Left Diagonal:** Is there a queen diagonally down-left?

If the cell is safe, we place the queen, recursively move to the next column, and **backtrack** by removing the queen when we return.

### JavaScript Code

```javascript
/**
 * Standard Backtracking Approach
 * Uses loops to iteratively check the safety of a cell before placing a queen.
 */
function solveNQueens(n) {
    let ans = [];
    // Initialize an N x N board filled with '.'
    let board = Array.from({ length: n }, () => new Array(n).fill('.'));

    // Helper function to check if a cell is safe from previous queens
    function isSafe(row, col, board, n) {
        let dupRow = row;
        let dupCol = col;

        // Check Upper-Left Diagonal
        while (row >= 0 && col >= 0) {
            if (board[row][col] === 'Q') return false;
            row--;
            col--;
        }

        row = dupRow;
        col = dupCol;
        // Check Straight Left Row
        while (col >= 0) {
            if (board[row][col] === 'Q') return false;
            col--;
        }

        row = dupRow;
        col = dupCol;
        // Check Lower-Left Diagonal
        while (row < n && col >= 0) {
            if (board[row][col] === 'Q') return false;
            row++;
            col--;
        }

        return true;
    }

    function solve(col, board, ans, n) {
        // Base case: If we successfully placed queens in all columns
        if (col === n) {
            ans.push(board.map(r => r.join('')));
            return;
        }

        // Try placing a queen in every row for the current column
        for (let row = 0; row < n; row++) {
            if (isSafe(row, col, board, n)) {
                // Pick: Place the Queen
                board[row][col] = 'Q';
                
                // Recurse to the next column
                solve(col + 1, board, ans, n);
                
                // Backtrack: Remove the Queen
                board[row][col] = '.';
            }
        }
    }

    solve(0, board, ans, n);
    return ans;
}

// Example Usage
console.log("Standard Backtracking (N=4):", solveNQueens(4)); 
```

### Complexity
* **Time Complexity:** $O(N! \times N)$. We are exploring $N!$ permutations for placing the queens. For every placement, the `isSafe` function takes $O(N)$ time to iterate through the board.
* **Space Complexity:** $O(N^2)$ for the board representation + $O(N)$ for the recursive call stack.

---

## 2️⃣ Optimal Approach: Backtracking with Hashing

We can optimize the time complexity by completely removing the $O(N)$ loop inside the `isSafe` function. Instead of looping to check if a row or diagonal is attacked, we can use **Hash Arrays** to keep track of which rows and diagonals currently have queens.

**The Mathematical Trick:**
1. **Left Row:** We use an array of size $N$. If `leftRow[row] === 1`, the row is taken.
2. **Lower Diagonal:** If you observe an $N \times N$ matrix, every cell in the same bottom-left to top-right diagonal shares the exact same sum of `row + col`. We use an array of size $2N - 1$.
3. **Upper Diagonal:** Every cell in the same top-left to bottom-right diagonal shares the exact same difference of `col - row`. To avoid negative indices, we add $N - 1$, meaning the index is `(N - 1) + (col - row)`. We use an array of size $2N - 1$.

By checking these three arrays, we can determine if a cell is safe in **$O(1)$ time**.

### JavaScript Code

```javascript
/**
 * Optimal Backtracking Approach (Hashing)
 * Uses Hash Arrays to check for attacking queens in O(1) time.
 */
function solveNQueensOptimal(n) {
    let ans = [];
    let board = Array.from({ length: n }, () => new Array(n).fill('.'));

    // Hash Arrays to keep track of attacked lines
    let leftRow = new Array(n).fill(0);
    let upperDiagonal = new Array(2 * n - 1).fill(0);
    let lowerDiagonal = new Array(2 * n - 1).fill(0);

    function solve(col, board, ans, n) {
        if (col === n) {
            ans.push(board.map(r => r.join('')));
            return;
        }

        for (let row = 0; row < n; row++) {
            // O(1) Check using our Hash Arrays
            if (
                leftRow[row] === 0 && 
                lowerDiagonal[row + col] === 0 && 
                upperDiagonal[n - 1 + col - row] === 0
            ) {
                // Pick: Place the Queen and mark the lines as attacked
                board[row][col] = 'Q';
                leftRow[row] = 1;
                lowerDiagonal[row + col] = 1;
                upperDiagonal[n - 1 + col - row] = 1;

                // Recurse to the next column
                solve(col + 1, board, ans, n);

                // Backtrack: Remove the Queen and unmark the lines
                board[row][col] = '.';
                leftRow[row] = 0;
                lowerDiagonal[row + col] = 0;
                upperDiagonal[n - 1 + col - row] = 0;
            }
        }
    }

    solve(0, board, ans, n);
    return ans;
}

// Example Usage
console.log("Optimal Hashing (N=4):", solveNQueensOptimal(4));
```

### Complexity
* **Time Complexity:** $O(N!)$. We still explore the permutations, but checking if a cell is safe now takes strictly $O(1)$ time instead of $O(N)$.
* **Space Complexity:** $O(N^2)$ for the board + $O(N)$ for the three hash arrays + $O(N)$ for the recursion stack.