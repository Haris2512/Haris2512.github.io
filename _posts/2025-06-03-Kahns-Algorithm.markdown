---
title: "Kahn's Algorithm"
date: 2025-06-03 00:00:00 +0700
categories: [Algorithms, Graph Traversal]
tags: [Kahn's Algorithm, Topological Sort, DAG, Dependency Resolution]
---

# Kahn's Algorithm

**Kahn's Algorithm** is a topological sorting algorithm designed for **Directed Acyclic Graphs (DAGs)** to determine a valid order of nodes based on their dependencies. It employs a **Breadth-First Search (BFS)**-based approach, processing nodes with no incoming edges (in-degree = 0) first, ensuring that for every directed edge \( u \rightarrow v \), node \( u \) appears before \( v \) in the sequence.

## Concept Overview
- **Principle**: Kahn's algorithm iteratively removes nodes with no dependencies (in-degree = 0) and updates the in-degrees of their neighbors, continuing until all nodes are processed or a cycle is detected.
- **Key Characteristics**:
  - Uses a queue to manage nodes with in-degree 0.
  - Detects cycles if the number of processed nodes is less than the total nodes.
  - Generates one or more valid topological orders.
- **Applications**:
  - Task scheduling based on dependencies.
  - Course prerequisite ordering.
  - Build system dependency resolution.
  - Cycle detection in directed graphs.

## When is Kahn's Algorithm Needed?
- Processing dependencies in the correct order.
- Detecting cycles in directed graphs.
- Scheduling tasks with prerequisites.

## Algorithm Steps
1. Compute the **in-degree** (number of incoming edges) for each node.
2. Add all nodes with in-degree = 0 to a queue.
3. While the queue is not empty:
   - Dequeue a node \( u \).
   - Add \( u \) to the topological order.
   - Reduce the in-degree of all neighbors of \( u \) by 1.
   - If any neighbor’s in-degree becomes 0, enqueue it.
4. If all nodes are processed, the graph is acyclic; otherwise, a cycle exists.

### Example
**Course Dependencies**:
- A and B are prerequisites for C.
- C is a prerequisite for D.
- **Valid Topological Order**: A, B, C, D or B, A, C, D.

**Project Task Dependencies**:
- A → D, F → A, F → B, B → D, D → C.
- **Valid Topological Order**: F, A, B, D, C or F, B, A, D, C.

## C++ Implementation
The provided code implements Kahn's algorithm for topological sorting. Below is a corrected and complete version:

```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
#include <queue>
#include <set>
using namespace std;

unordered_map<char, vector<char>> graph;
unordered_map<char, int> in_degree;
set<char> vertices;

void topologicalSort() {
    queue<char> q;
    // Add all nodes with in-degree 0 to queue
    for (char v : vertices) {
        if (in_degree[v] == 0) {
            q.push(v);
        }
    }

    vector<char> result;
    while (!q.empty()) {
        char u = q.front();
        q.pop();
        result.push_back(u);

        for (char v : graph[u]) {
            in_degree[v]--;
            if (in_degree[v] == 0) {
                q.push(v);
            }
        }
    }

    // Output result
    for (char v : result) {
        cout << v << " ";
    }
    cout << endl;

    // Check for cycle
    if (result.size() != vertices.size()) {
        cout << "Graph contains a cycle!" << endl;
    }
}

int main() {
    int E;
    cout << "Masukkan jumlah edge (sisi): ";
    cin >> E;

    cout << "Masukkan sisi dalam format 'A B' (tanpa panah):" << endl;
    for (int i = 0; i < E; ++i) {
        char u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        in_degree[v]++;
        vertices.insert(u);
        vertices.insert(v);
    }

    // Initialize in-degree for unconnected nodes
    for (char v : vertices) {
        if (in_degree.find(v) == in_degree.end()) {
            in_degree[v] = 0;
        }
    }

    topologicalSort();
    return 0;
}
```

**Sample Input**:
```
5
F A
F B
A D
B D
D C
```

**Output**:
```
F A B D C
```

## Algorithm Visualization
The provided image illustrates a backtracking process for a maze, which is unrelated to Kahn's algorithm. However, we can adapt a similar visualization concept for Kahn's algorithm using a topological sort example:

- **Graph**: A → D, F → A, F → B, B → D, D → C.
- **Process**:
  - Start: In-degrees: A(1), B(1), C(1), D(2), F(0).
  - Enqueue F (in-degree = 0), process F → A, B.
  - Update: A(0), B(0), C(1), D(2), F(processed).
  - Enqueue A, B, process A → D, B → D.
  - Update: C(1), D(0), A(processed), B(processed).
  - Enqueue D, process D → C.
  - Update: C(0), D(processed).
  - Enqueue C, process C.
  - Result: F, A, B, D, C.

## Time and Space Complexity
- **Time Complexity**: \( O(V + E) \), where \( V \) is the number of vertices and \( E \) is the number of edges.
- **Space Complexity**: \( O(V) \) for the queue, in-degree map, and graph.

## Strengths and Limitations
- **Strengths**:
  - Efficient with \( O(V + E) \) complexity.
  - Detects cycles in directed graphs.
  - Ideal for dependency-based scheduling.
- **Limitations**:
  - Only applicable to DAGs; fails with cycles.
  - Requires additional data structures (in-degree and graph).
  - May produce multiple valid orders.

## Conclusion
Kahn's Algorithm is an efficient and systematic method for topological sorting in DAGs, leveraging in-degree and a queue to resolve dependencies. Its \( O(V + E) \) complexity makes it suitable for real-world applications like course scheduling, project management, and build systems. The C++ implementation demonstrates its practicality in handling graph-based dependency problems.

---
**References**:  
- Group 9 Presentation, 03 June 2025.