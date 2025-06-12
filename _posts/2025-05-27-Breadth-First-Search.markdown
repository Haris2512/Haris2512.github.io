---
title: "Breadth-First Search (BFS)"
date: 2025-05-27 00:00:00 +0800
categories: [Algorithms, Graph Traversal]
tags: [Breadth-First Search, BFS, Pathfinding]
---

# Breadth-First Search (BFS)

**Breadth-First Search (BFS)** is a graph traversal algorithm that explores nodes level by level, starting from a given node and visiting all its neighbors before moving to nodes further away. It uses a **queue** to ensure nodes are processed in order of their distance from the start node, making it ideal for finding the shortest path in unweighted graphs.

## Concept Overview
- **Principle**: BFS explores all nodes at the current depth (level) before moving to the next depth, resembling a "spreading out" approach.
- **Key Characteristics**:
  - Uses a queue for level-order traversal.
  - Marks visited nodes to avoid cycles.
  - Guarantees the shortest path in unweighted graphs.
- **Applications**:
  - Finding the shortest path in mazes (e.g., Pac-Man).
  - Social network analysis (e.g., "people you may know").
  - Network topology mapping (e.g., discovering devices).
  - Navigation systems (e.g., Google Maps, finding routes with minimal intersections).

## Algorithm Steps
1. Initialize a queue and mark the start node as visited.
2. Enqueue the start node.
3. While the queue is not empty:
   - Dequeue a node (current node).
   - If it is the goal node, record the solution.
   - Enqueue all unvisited neighbors, marking them as visited.
4. Repeat until the goal is found or the queue is empty.

### Pseudocode
```
BFS(graph, start, goal)
    queue = empty queue
    visited = empty set
    enqueue(queue, start)
    add start to visited
    while queue is not empty
        node = dequeue(queue)
        if node is goal
            return path or solution
        for each neighbor in graph[node]
            if neighbor not in visited
                add neighbor to visited
                enqueue(queue, neighbor)
    return no path found
```

## C++ Implementation
Below is a C++ implementation of BFS for finding the shortest path in a graph represented as an adjacency list:

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <unordered_map>
#include <string>

using namespace std;

// BFS to find shortest path from start to goal
void bfs(unordered_map<char, vector<char>>& graph, char start, char goal) {
    queue<char> q;
    unordered_map<char, bool> visited;
    unordered_map<char, char> parent; // To reconstruct path
    vector<char> path;
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        char node = q.front();
        q.pop();
        
        // If goal is found, reconstruct path
        if (node == goal) {
            cout << "Path found: ";
            char current = goal;
            while (current != start) {
                path.push_back(current);
                current = parent[current];
            }
            path.push_back(start);
            for (int i = path.size() - 1; i >= 0; i--) {
                cout << path[i] << (i > 0 ? " -> " : "");
            }
            cout << endl;
            return;
        }
        
        // Explore neighbors
        for (char neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                parent[neighbor] = node;
                q.push(neighbor);
            }
        }
    }
    
    cout << "No path found from " << start << " to " << goal << endl;
}

int main() {
    // Example graph (undirected)
    unordered_map<char, vector<char>> graph = {
        {'S', {'A', 'B'}},
        {'A', {'S', 'C'}},
        {'B', {'S', 'C', 'D'}},
        {'C', {'A', 'B', 'D', 'E'}},
        {'D', {'B', 'C', 'E', 'F'}},
        {'E', {'C', 'D', 'F'}},
        {'F', {'D', 'E', 'H'}},
        {'H', {'F', 'G'}},
        {'G', {'H'}}
    };
    
    char start = 'S';
    char goal = 'G';
    bfs(graph, start, goal);
    
    return 0;
}
```

**Output** for the example:
```
Path found: S -> B -> G
```

## Algorithm Visualization
Below is a diagram illustrating the BFS traversal for the example graph starting from node `S` to find `G`:

```mermaid
graph TD
    S --> A
    S --> B
    A --> C
    B --> C
    B --> D
    C --> D
    C --> E
    D --> E
    D --> F
    E --> F
    F --> H
    H --> G
    style S fill:#f9f,stroke:#333,stroke-width:2px
    style G fill:#bbf,stroke:#333,stroke-width:2px
```

**Queue Progression**:
- Start: `S`
- Dequeue `S`: Enqueue `A, B` → Queue: `A, B`
- Dequeue `A`: Enqueue `C` → Queue: `B, C`
- Dequeue `B`: Enqueue `D` → Queue: `C, D`
- Dequeue `C`: Enqueue `E` → Queue: `D, E`
- Dequeue `D`: Enqueue `F` → Queue: `E, F`
- Dequeue `E`: Queue: `F`
- Dequeue `F`: Enqueue `H` → Queue: `H`
- Dequeue `H`: Enqueue `G` → Queue: `G`
- Dequeue `G`: Goal found, path: `S -> B -> G`

## Time and Space Complexity
- **Time Complexity**: `O(V + E)`, where `V` is the number of vertices and `E` is the number of edges, as each vertex and edge is processed once.
- **Space Complexity**:
  - Queue: `O(V)` in the worst case.
  - Visited set and parent map: `O(V)`.
  - Total: `O(V)`.

## Strengths and Limitations
- **Strengths**:
  - Guarantees the shortest path in unweighted graphs.
  - Complete (finds a solution if one exists).
  - Simple to implement with a queue.
- **Limitations**:
  - High memory usage for large graphs due to queue and visited set.
  - Inefficient for weighted graphs (use Dijkstra’s algorithm instead).
  - May explore unnecessary nodes in dense graphs.

## Applications
- **Maze Solving**: Finds the shortest path in games like Pac-Man.
- **Social Networks**: Identifies connections within a certain degree (e.g., "people you may know").
- **Network Analysis**: Maps devices in a network (e.g., routers, PCs).
- **Navigation Systems**: Finds routes with minimal intersections in GPS applications like Google Maps.

## Conclusion
Breadth-First Search (BFS) is a versatile graph traversal algorithm that explores nodes level by level, ensuring the shortest path in unweighted graphs. With a time complexity of `O(V + E)` and space complexity of `O(V` , it is effective for applications like maze solving, social network analysis, and navigation systems. Its simplicity and optimality make it a fundamental tool in algorithm design.

---
**References**:  
- Group 7 Presentation, 27 May 2025.