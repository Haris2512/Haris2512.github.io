---
title: "Dijkstra's Algorithm"
date: 2025-06-03 00:00:00 +0700
categories: [Algorithms, Graph Traversal]
tags: [Dijkstra's Algorithm, Shortest Path, Graph, Optimization]
---

# Dijkstra's Algorithm

**Dijkstra's Algorithm** is a method for finding the shortest path between two nodes in a graph, which consists of nodes (vertices) and edges (connections) with assigned weights. This algorithm determines the optimal route from a starting node to a destination node, assuming all edge weights are non-negative.

## Concept Overview
- **Principle**: Uses a greedy approach, selecting the locally optimal path at each step to reach the globally shortest path.
- **Key Characteristics**:
  - Works with weighted graphs where edge weights are non-negative.
  - Maintains a set of unvisited nodes and updates the shortest distance to each node iteratively.
- **Applications**:
  - Road network navigation (e.g., GPS).
  - Package delivery route optimization.
  - Network routing protocols.

## How Dijkstra's Algorithm Works
1. Create a table with columns for distances from the starting node and rows for each node.
2. Select the starting node and destination node, ensuring they are directly connected.
3. Calculate distances to adjacent nodes.
4. Choose the shortest distance and use that node as the reference for the next step.
5. Repeat until all nodes are processed or the destination is reached.

### Example Problem
**Find the shortest path and distance from node A to F:**

```
Graph:
A --1--> B --2--> D --2--> F
|       |       |
2       3       3
|       |       |
v       v       v
C --3--> E --1--> F

Initial Table:
V  A  B  C  D  E  F
A  0  ∞  ∞  ∞  ∞  ∞
B  0  ∞
E  0
C  0
D  0

Steps:
- A to B: 1, A to C: 2
- From B: B to D: 3, B to E: 3
- From C: C to E: 3
- From D: D to F: 2
- From E: E to F: 1
- Shortest path: A → B → D → F or A → B → E → F = 4
```

**Shortest Path**: ABEF or ABDF = 4.

## C++ Implementation
Below is a basic implementation of Dijkstra's algorithm:

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <limits>
using namespace std;

#define INF numeric_limits<int>::max()

vector<int> dijkstra(vector<vector<pair<int, int>>>& graph, int start, int V) {
    vector<int> dist(V, INF);
    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
    
    dist[start] = 0;
    pq.push({0, start});

    while (!pq.empty()) {
        int u = pq.top().second;
        pq.pop();

        for (auto& edge : graph[u]) {
            int v = edge.first;
            int weight = edge.second;

            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.push({dist[v], v});
            }
        }
    }
    return dist;
}

int main() {
    int V = 6; // Nodes A=0, B=1, C=2, D=3, E=4, F=5
    vector<vector<pair<int, int>>> graph(V);

    // Add edges (weight)
    graph[0].push_back({1, 1}); // A->B
    graph[0].push_back({2, 2}); // A->C
    graph[1].push_back({3, 2}); // B->D
    graph[1].push_back({4, 3}); // B->E
    graph[2].push_back({4, 3}); // C->E
    graph[3].push_back({5, 2}); // D->F
    graph[4].push_back({5, 1}); // E->F

    vector<int> distances = dijkstra(graph, 0, V);
    cout << "Shortest distances from A:" << endl;
    char nodes[] = {'A', 'B', 'C', 'D', 'E', 'F'};
    for (int i = 0; i < V; i++) {
        cout << nodes[i] << ": " << (distances[i] == INF ? "INF" : to_string(distances[i])) << endl;
    }
    return 0;
}
```

**Output**:
```
Shortest distances from A:
A: 0
B: 1
C: 2
D: 3
E: 3
F: 4
```

## Real-World Examples
1. **Navigation**: Finding the shortest route from home to school using GPS.
2. **Package Delivery**: Optimizing delivery routes to save fuel and time.

## Advantages
- Guarantees the shortest path for non-negative edge weights.
- Efficient for dense graphs and widely used in GPS and network routing.
- Computes shortest paths to all nodes from a single source.

## Limitations
- Fails with negative edge weights.
- Less efficient for large, sparse graphs without optimization (e.g., priority queue).
- Computes unnecessary paths to all nodes, unlike A* for specific destinations.

## Conclusion
Dijkstra's Algorithm is a highly effective and widely used method for finding the shortest path in weighted graphs with non-negative edges. Its greedy approach, combined with optimizations like priority queues, makes it ideal for applications in navigation, logistics, and network design. Despite its limitations with negative weights and sparse graphs, it remains a cornerstone in graph theory and practical problem-solving.

---
**References**:  
- Group 10 Presentation, 3 June 2025.