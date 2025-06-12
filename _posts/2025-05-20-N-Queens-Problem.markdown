---
title: "N-Queens Problem"
date: 2025-05-20 00:00:00 +0800
categories: [Algorithms, Backtracking]
tags: [N-Queens, Backtracking, Constraint Satisfaction]
---

# N-Queens Problem

The **N-Queens Problem** is a classic combinatorial problem in computer science and mathematics, involving the placement of `N` queens on an `N × N` chessboard such that no two queens attack each other. In chess, a queen can move horizontally, vertically, or diagonally, so no two queens can share the same row, column, or diagonal. The 8-Queens problem (on an 8x8 board) is a well-known instance of this problem.

## Problem Overview
- **Input**: An `N × N` chessboard and `N` queens.
- **Objective**: Place all `N` queens on the board such that no two queens threaten each other.
- **Constraints**: No two queens can be in the same row, column, or diagonal.
- **Purpose**:
  - Find all valid configurations.
  - Develop and test search and optimization algorithms (e.g., backtracking, DFS, heuristics).
  - Serve as a model for constraint satisfaction problems (CSP).

## Importance
The N-Queens Problem is significant in various contexts:
- **AI and Algorithm Study**: A standard example for teaching backtracking, branch and bound, heuristic, and metaheuristic algorithms.
- **Constraint Satisfaction Problems (CSP)**: Represents a classic CSP where variables (queen positions) must satisfy constraints (no attacks).
- **Real-World Applications**:
  - Task scheduling with conflict avoidance.
  - Circuit design for placing non-interfering modules.
  - Resource allocation with mutually exclusive constraints.
- **Complexity Analysis**: Demonstrates exponential growth in complexity as `N` increases, ideal for evaluating algorithm scalability.

## Backtracking Algorithm
The N-Queens Problem is typically solved using **backtracking**, a systematic exhaustive search that builds solutions incrementally and backtracks when a partial solution is invalid. The algorithm:
1. Places a queen in each row, one at a time, starting from the first row.
2. For each row, tries placing a queen in each column, checking if the position is safe (not attacked by previously placed queens).
3. If a safe position is found, proceeds to the next row.
4. If no safe position exists, backtracks to the previous row and tries a different column.
5. Continues until all queens are placed or all possibilities are exhausted.

### Pseudocode
```
N-QUEENS(N)
    board = empty N × N board
    SOLVE-N-QUEENS(board, row = 0)
    return solutions

SOLVE-N-QUEENS(board, row)
    if row == N
        record solution
        return
    for col = 0 to N-1
        if IS-SAFE(board, row, col)
            place queen at board[row][col]
            SOLVE-N-QUEENS(board, row + 1)
            remove queen from board[row][col] // Backtrack
```

### Safety Check
A position `(row, col)` is safe if:
- No queen exists in the same column.
- No queen exists on the diagonals (checked by ensuring `|row - row'| ≠ |col - col'|` for all previously placed queens).

## C++ Implementation
Below is a C++ implementation of the N-Queens Problem using backtracking:

```cpp
#include <iostream>
#include <vector>

using namespace std;

// Function to check if a position is safe
bool isSafe(vector<int>& board, int row, int col, int N) {
    for (int prevRow = 0; prevRow < row; prevRow++) {
        int prevCol = board[prevRow];
        // Check same column
        if (prevCol == col) return false;
        // Check diagonals
        if (abs(prevRow - row) == abs(prevCol - col)) return false;
    }
    return true;
}

// Function to print the board
void printBoard(vector<int>& board, int N) {
    for (int i = 0; i < N; i++) {
        for (int j = 0; j < N; j++) {
            cout << (board[i] == j ? "Q " : ". ");
        }
        cout << endl;
    }
    cout << endl;
}

// Recursive function to solve N-Queens
void solveNQueens(vector<int>& board, int row, int N, int& solutionCount) {
    if (row == N) {
        // Found a valid solution
        solutionCount++;
        cout << "Solution " << solutionCount << ":\n";
        printBoard(board, N);
        return;
    }
    for (int col = 0; col < N; col++) {
        if (isSafe(board, row, col, N)) {
            board[row] = col; // Place queen
            solveNQueens(board, row + 1, N, solutionCount);
            board[row] = -1; // Backtrack
        }
    }
}

// Main function to initiate N-Queens solver
void nQueens(int N) {
    vector<int> board(N, -1); // board[i] stores column of queen in row i
    int solutionCount = 0;
    solveNQueens(board, 0, N, solutionCount);
    cout << "Total solutions for N = " << N << ": " << solutionCount << endl;
}

int main() {
    int N = 4; // Example for 4-Queens
    nQueens(N);
    return 0;
}
```

**Output** for `N = 4`:
```
Solution 1:
. Q . . 
. . . Q 
Q . . . 
. . Q . 

Solution 2:
. . Q . 
Q . . . 
. . . Q 
. Q . . 

Total solutions for N = 4: 2
```

## Algorithm Visualization
Below is a diagram illustrating the backtracking process for the 4-Queens problem, showing the exploration of possible queen placements:

```mermaid
graph TD
    A[Start: Empty Board] --> B[Row 0: Try Q at Col 0]
    B --> C[Row 1: Try Q at Col 2]
    C --> D[Row 2: Try Q at Col 0]
    D --> E[Row 3: Try Q at Col 3]
    E --> F[Solution 1]
    E --> G[Backtrack: No more valid cols in Row 3]
    D --> H[Backtrack: Try next col in Row 2]
    C --> I[Backtrack: Try next col in Row 1]
    I --> J[Row 1: Try Q at Col 3]
    J --> K[Row 2: Try Q at Col 1]
    K --> L[Row 3: Try Q at Col 0]
    L --> M[Solution 2]
    L --> N[Backtrack: No more valid cols in Row 3]
    K --> O[Backtrack: Try next col in Row 2]
```

The diagram represents the search tree, where each node is a queen placement attempt, and backtracking occurs when no valid placement is found.

## Time and Space Complexity
- **Time Complexity**: 
  - Worst case: `O(N!)` due to exploring all possible permutations of queen placements.
  - Backtracking prunes invalid branches, making it more efficient than brute force but still exponential.
- **Space Complexity**: 
  - Board storage: `O(N)` for the board array.
  - Recursion stack: `O(N)` for the call stack.
  - Total: `O(N)`.

## Applications
- **Task Scheduling**: Avoiding conflicts in resource allocation.
- **Circuit Design**: Placing non-interfering components in VLSI design.
- **Resource Allocation**: Assigning mutually exclusive resources in systems like network bandwidth or employee seating arrangements.

## Strengths and Limitations
- **Strengths**:
  - Systematic approach to explore all valid solutions.
  - Effective for CSPs and combinatorial problems.
  - Teaches optimization techniques like pruning invalid branches.
- **Limitations**:
  - Exponential time complexity for large `N`.
  - May require advanced techniques (e.g., forward checking, heuristics) for scalability.

## Real-World Analogy
The N-Queens Problem is analogous to assigning `N` employees to `N` workspaces in an office, ensuring no two employees are in adjacent or conflicting positions (e.g., same row, column, or diagonal in a grid-like office layout). Backtracking ensures valid placements by retrying different configurations when conflicts arise.

## Conclusion
The N-Queens Problem is a fundamental example of a constraint satisfaction problem solved efficiently using backtracking. It serves as a powerful teaching tool in algorithm design and analysis, with applications in scheduling, circuit design, and resource allocation. Despite its exponential complexity, backtracking optimizes the search by pruning invalid paths, making it practical for moderate values of `N`.

---
**References**:  
- Group 4 Presentation, 20 May 2025.