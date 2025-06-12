---
title: "Depth-First Search (DFS)"
date: 2025-05-27 00:00:00 +0800
categories: [Algorithms, Graph Traversal]
tags: [Depth-First Search, DFS, Graph, Pathfinding]
---

# Depth-First Search (DFS)

**Depth-First Search (DFS)** is a graph traversal algorithm that explores as far as possible along each branch before backtracking. It uses a **stack** (either explicitly or via recursion) to follow the **Last In, First Out (LIFO)** principle, diving deep into the graph from the starting node before exploring other branches. DFS is particularly useful for tasks requiring deep exploration, such as finding paths in mazes or detecting cycles in graphs.

## Concept Overview
- **Principle**: DFS starts at a root node, explores one of its children as deeply as possible, then backtracks to explore other children. It continues until all nodes are visited or a goal is found.
- **Key Characteristics**:
  - Uses a stack (implicit via recursion or explicit).
  - Explores one path to its deepest point before backtracking.
  - Suitable for finding all possible paths or checking connectivity.
- **Applications**:
  - Maze solving and pathfinding.
  - Cycle detection in graphs.
  - Topological sorting in directed acyclic graphs (DAGs).
  - Connected component analysis in networks.

## Algorithm Steps
1. Start at the root node and mark it as visited.
2. Push the node onto a stack (or use recursion).
3. While the stack is not empty:
   - Pop a node from the stack (or process the current node in recursion).
   - If it is the goal node, record the solution.
   - For each unvisited neighbor, mark it as visited and push it onto the stack (or recurse).
4. Backtrack when no unvisited neighbors remain.
5. Repeat until the goal is found or all nodes are explored.

### Pseudocode
```
DFS(graph, start, goal)
    stack = empty stack
    visited = empty set
    push(stack, start)
    add start to visited
    while stack is not empty
        node = pop(stack)
        if node is goal
            return path or solution
        for each neighbor in graph[node]
            if neighbor not in visited
                add neighbor to visited
                push(stack, neighbor)
    return no path found
```

## C++ Implementation
The document includes a partial C++ code snippet on page 9, which appears to describe a graph and a DFS traversal starting from a node. Below is a corrected and complete implementation based on the provided snippet:

```cpp
#include <iostream>
#include <vector>
using namespace std;

// DFS function to print traversal
void DFS(int node, vector<vector<int>>& graph, vector<bool>& visited) {
    // Mark current node as visited and print it
    visited[node] = true;
    cout << node << " ";

    // Explore all neighbors
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            DFS(neighbor, graph, visited);
        }
    }
}

int main() {
    int n = 6; // Number of nodes
    // Initialize adjacency list for the graph
    vector<vector<int>> graph(n);
    
    // Add edges based on page 9 (corrected for clarity)
    graph[0].push_back(1);
    graph[1].push_back(0);
    graph[1].push_back(2);
    graph[1].push_back(3);
    graph[1].push_back(4);
    graph[2].push_back(0);
    graph[2].push_back(5);
    graph[3].push_back(1);
    graph[4].push_back(1);
    graph[5].push_back(2);
    
    vector<bool> visited(n, false);
    
    cout << "DFS traversal starting from node 1: ";
    DFS(1, graph, visited);
    cout << endl;
    
    return 0;
}
```

**Output** (based on possible traversal):
```
DFS traversal starting from node 1: 1 0 2 5 3 4
```

**Note**: The exact order of nodes depends on the order of neighbors in the adjacency list. The provided code assumes a recursive DFS implementation, which is standard and aligns with the document’s description of using a stack implicitly via recursion.

## Algorithm Visualization
The document on pages 5 and 6 provides example DFS traversals for a graph with nodes labeled numerically. Assuming a graph with nodes 1 to 10 and edges inferred from the traversal orders (e.g., `1, 2, 4, 5, 6, 7` and `1, 2, 4, 6, 9, 10`), we can represent a possible graph structure:

```mermaid
graph TD
    1 --> 2
    2 --> 4
    2 --> 5
    4 --> 6
    5 --> 7
    6 --> 9
    9 --> 10
    style 1 fill:#f9f,stroke:#333,stroke-width:2px
    style 10 fill:#bbf,stroke:#333,stroke-width:2px
```

**DFS Traversal Example** (starting from node 1):
- Visit 1, explore 2.
- From 2, explore 4, then 6, then 9, then 10 (goal reached: `1, 2, 4, 6, 9, 10`).
- Backtrack to 2, explore 5, then 7 (another path: `1, 2, 5, 7`).

## Time and Space Complexity
- **Time Complexity**: `O(V + E)`, where `V` is the number of vertices and `E` is the number of edges, as each vertex and edge is processed once.
- **Space Complexity**:
  - Recursion stack or explicit stack: `O(V)` for the deepest path.
  - Visited set: `O(V)`.
  - Total: `O(V)`.

## Strengths and Limitations
The document does not explicitly list strengths and limitations, but based on standard DFS properties and context from page 7 (corrupted as "RELEBEH"), we can infer:
- **Strengths**:
  - Memory-efficient due to lower space requirements compared to BFS.
  - Suitable for deep exploration (e.g., maze solving, cycle detection).
  - Can find all possible paths in a graph.
  - Simple to implement recursively.
- **Limitations**:
  - Does not guarantee the shortest path (unlike BFS).
  - Risk of stack overflow for very deep graphs.
  - May get stuck in infinite loops in cyclic graphs without proper visited checks.
  - Traversal order depends on neighbor processing order.

## Applications
Based on the context of DFS and its comparison to BFS and backtracking:
- **Maze Solving**: Explores deep paths to find exits (e.g., as in the Rat in a Maze problem).
- **Cycle Detection**: Identifies cycles in graphs (e.g., in dependency graphs).
- **Topological Sorting**: Orders nodes in DAGs (e.g., for task scheduling).
- **Network Analysis**: Explores connected components in social or computer networks.

## Conclusion
Depth-First Search (DFS) is a powerful algorithm for deep exploration of graphs, using a stack-based approach (implicitly via recursion or explicitly). It is memory-efficient and suitable for tasks like pathfinding, cycle detection, and topological sorting. While it does not guarantee the shortest path, its simplicity and flexibility make it a fundamental tool in graph algorithms. The provided C++ implementation and example traversals demonstrate its application in exploring graph structures.

---
**References**:  
- Group 8 Presentation, 27 May 2025.