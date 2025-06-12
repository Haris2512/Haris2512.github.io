---
title: "Rat in a Maze Problem"
date: 2025-05-27 00:00:00 +0800
categories: [Algorithms, Backtracking]
tags: [Rat in a Maze, Backtracking, Pathfinding]
---

# Rat in a Maze Problem

The **Rat in a Maze Problem** is a classic algorithmic problem where a rat must navigate from the top-left corner `(0, 0)` of an `N × N` maze to the bottom-right corner `(N-1, N-1)`. The maze is represented as a matrix where `1` indicates a traversable path and `0` indicates a wall. The rat can move in four directions (up, down, left, right), and the solution uses **backtracking** to explore all possible paths, backtracking when a path leads to a dead end.

## Problem Overview
- **Input**: An `N × N` matrix where `1` = traversable, `0` = wall.
- **Objective**: Find all possible paths from `(0, 0)` to `(N-1, N-1)` without revisiting cells.
- **Constraints**:
  - Moves: Up (`U`), Down (`D`), Left (`L`), Right (`R`).
  - Stay within maze boundaries.
  - Only move to cells with `1`.
  - Each cell can be visited at most once.
- **Applications**:
  - Robot navigation in constrained environments.
  - GPS and pathfinding algorithms.
  - Game design and simulations.

## Backtracking Algorithm
The algorithm systematically explores paths:
1. Start at `(0, 0)` and check if the position is valid.
2. Try moving in each direction (R, D, L, U) if the move is valid (within bounds, cell = `1`, not visited).
3. Mark the current cell as visited and include the move in the path.
4. Recursively explore the next position.
5. If the destination `(N-1, N-1)` is reached, record the path.
6. If a path leads to a dead end, backtrack by unmarking the cell and trying another direction.

### Pseudocode
```
RAT-IN-MAZE(maze, N)
    solution = empty N × N matrix
    path = empty string
    SOLVE-MAZE(maze, 0, 0, N, solution, path)
    return all paths

SOLVE-MAZE(maze, x, y, N, solution, path)
    if x == N-1 and y == N-1
        record path
        return
    for each direction in {R, D, L, U}
        next_x, next_y = new coordinates
        if IS-VALID(maze, next_x, next_y, N, solution)
            solution[next_x][next_y] = 1
            path += direction
            SOLVE-MAZE(maze, next_x, next_y, N, solution, path)
            solution[next_x][next_y] = 0 // Backtrack
            remove last direction from path
```

### Validity Check
A move to `(x, y)` is valid if:
- `(x, y)` is within bounds (`0 ≤ x, y < N`).
- `maze[x][y] = 1`.
- The cell has not been visited (`solution[x][y] = 0`).

## C++ Implementation
Below is a C++ implementation of the Rat in a Maze Problem:

```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Check if move is valid
bool isValid(vector<vector<int>>& maze, int x, int y, int N, vector<vector<int>>& solution) {
    return (x >= 0 && x < N && y >= 0 && y < N && maze[x][y] == 1 && solution[x][y] == 0);
}

// Recursive function to find all paths
void solveMaze(vector<vector<int>>& maze, int x, int y, int N, vector<vector<int>>& solution, string path, vector<string>& paths) {
    // Base case: reached destination
    if (x == N - 1 && y == N - 1) {
        paths.push_back(path);
        return;
    }
    
    // Possible moves: Right, Down, Left, Up
    int dx[] = {0, 1, 0, -1};
    int dy[] = {1, 0, -1, 0};
    char dir[] = {'R', 'D', 'L', 'U'};
    
    for (int i = 0; i < 4; i++) {
        int next_x = x + dx[i];
        int next_y = y + dy[i];
        
        if (isValid(maze, next_x, next_y, N, solution)) {
            solution[next_x][next_y] = 1; // Mark as visited
            solveMaze(maze, next_x, next_y, N, solution, path + dir[i], paths);
            solution[next_x][next_y] = 0; // Backtrack
        }
    }
}

// Main function to solve Rat in a Maze
void ratInMaze(vector<vector<int>>& maze, int N) {
    vector<string> paths;
    vector<vector<int>> solution(N, vector<int>(N, 0));
    
    // Start at (0, 0) if valid
    if (maze[0][0] == 1) {
        solution[0][0] = 1;
        solveMaze(maze, 0, 0, N, solution, "", paths);
    }
    
    // Print results
    if (paths.empty()) {
        cout << "No paths found\n";
    } else {
        cout << "Possible paths:\n";
        for (const string& path : paths) {
            cout << path << endl;
        }
    }
}

int main() {
    // Example maze
    vector<vector<int>> maze = {
        {1, 0, 0, 0},
        {1, 1, 0, 1},
        {1, 1, 0, 0},
        {0, 1, 1, 1}
    };
    int N = 4;
    
    ratInMaze(maze, N);
    return 0;
}
```

**Output** for the example:
```
Possible paths:
DRDDRR
DDRDRR
```

## Algorithm Visualization
Below is a diagram illustrating the backtracking process for the example maze:

```mermaid
graph TD
    A[Start: (0,0)] --> |D| B[(1,0)]
    B --> |R| C[(1,1)]
    C --> |D| D[(2,1)]
    D --> |D| E[(3,1)]
    E --> |R| F[(3,2)]
    F --> |R| G[(3,3) Solution: DRDDRR]
    E --> |Backtrack| H[Try other moves]
    D --> |Backtrack| I[Try other moves]
    C --> |Backtrack| J[Try other moves]
    B --> |D| K[(2,0)]
    K --> |R| L[(2,1)]
    L --> |D| M[(3,1)]
    M --> |R| N[(3,2)]
    N --> |R| O[(3,3) Solution: DDRDRR]
```

The diagram shows the exploration of paths, with backtracking when a move leads to a dead end.

## Time and Space Complexity
- **Time Complexity**: `O(4^(N^2))` in the worst case, as each cell can have up to 4 possible moves, and the maze has `N^2` cells. Backtracking prunes invalid paths, but complexity remains exponential.
- **Space Complexity**:
  - Solution matrix: `O(N^2)`.
  - Recursion stack: `O(N^2)` for the deepest path.
  - Total: `O(N^2)`.

## Strengths and Limitations
- **Strengths**:
  - Simple and easy to implement.
  - Finds all possible solutions.
  - Flexible for various maze configurations.
  - Requires minimal additional data structures.
- **Limitations**:
  - Inefficient for large mazes due to exponential complexity.
  - Risk of stack overflow for deep recursion.
  - May explore redundant paths if not optimized.
  - Not optimal for finding the shortest path (use BFS for that).

## Applications
- **Robot Navigation**: Path planning in constrained environments.
- **GPS Systems**: Finding routes in navigation applications.
- **Game Design**: Pathfinding in maze-based games or simulations.

## Conclusion
The Rat in a Maze Problem is an excellent demonstration of backtracking, systematically exploring all valid paths while pruning invalid ones. Despite its exponential time complexity, it is effective for small to moderate-sized mazes and has practical applications in navigation and game design. The provided C++ implementation finds all possible paths, making it a valuable tool for understanding recursive problem-solving.

---
**References**:  
- Group 6 Presentation, 27 May 2025.