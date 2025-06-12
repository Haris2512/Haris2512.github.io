---
title: "Fractional Knapsack Problem"
date: 2025-05-06 00:00:00 +0800
categories: [Algorithms, Greedy]
tags: [Fractional Knapsack, Greedy Algorithm, Optimization]
---

# Fractional Knapsack Problem

The **Fractional Knapsack Problem** is an optimization problem where the goal is to maximize the total value of items placed in a knapsack with a limited weight capacity. Unlike the 0/1 Knapsack Problem, where items must be taken entirely or not at all, the Fractional Knapsack Problem allows taking fractions of items, making it solvable efficiently using a **greedy algorithm**.

## Problem Overview
- **Input**: A set of items, each with a weight (`w`) and value (`v`), and a knapsack with a maximum weight capacity (`W`).
- **Objective**: Maximize the total value of items in the knapsack without exceeding the weight capacity, allowing fractional parts of items.
- **Key Idea**: Use a greedy approach by selecting items with the highest value-to-weight ratio (`v/w`) to maximize value within the capacity constraint.

## Greedy Algorithm
1. Calculate the value-to-weight ratio (`v/w`) for each item.
2. Sort items by their value-to-weight ratio in descending order.
3. Add items to the knapsack starting with the highest ratio, taking as much as possible until the knapsack is full.
4. If the remaining capacity is insufficient for an entire item, take a fraction of that item to fill the knapsack.

### Pseudocode
```
FRACTIONAL-KNAPSACK(w, v, W, n)
    Compute v[i]/w[i] for each item i
    Sort items by v[i]/w[i] in descending order
    Initialize total_value = 0, remaining_capacity = W
    for i = 1 to n
        if remaining_capacity >= w[i]
            Add entire item i to knapsack
            total_value += v[i]
            remaining_capacity -= w[i]
        else
            Add fraction of item i: (remaining_capacity / w[i]) * v[i]
            total_value += (remaining_capacity / w[i]) * v[i]
            break
    return total_value
```

## C++ Implementation
Below is a C++ implementation of the Fractional Knapsack Problem:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Structure to represent an item
struct Item {
    int value, weight;
    double ratio; // Value-to-weight ratio
};

// Comparator to sort items by ratio in descending order
bool compare(Item a, Item b) {
    return a.ratio > b.ratio;
}

// Function to compute maximum value for fractional knapsack
double fractionalKnapsack(vector<int> values, vector<int> weights, int capacity) {
    int n = values.size();
    vector<Item> items(n);
    
    // Calculate value-to-weight ratio for each item
    for (int i = 0; i < n; i++) {
        items[i] = {values[i], weights[i], (double)values[i] / weights[i]};
    }
    
    // Sort items by ratio in descending order
    sort(items.begin(), items.end(), compare);
    
    double total_value = 0.0;
    int remaining_capacity = capacity;
    
    // Process each item
    for (int i = 0; i < n; i++) {
        if (remaining_capacity >= items[i].weight) {
            // Take entire item
            total_value += items[i].value;
            remaining_capacity -= items[i].weight;
            cout << "Item " << i + 1 << ": Take all (" << items[i].weight << " units)\n";
        } else {
            // Take fraction of item
            double fraction = (double)remaining_capacity / items[i].weight;
            total_value += fraction * items[i].value;
            cout << "Item " << i + 1 << ": Take " << fraction * 100 << "% (" << remaining_capacity << " units)\n";
            break;
        }
    }
    
    return total_value;
}

int main() {
    // Example: Values, weights, and knapsack capacity
    vector<int> values = {60, 100, 120};
    vector<int> weights = {10, 20, 30};
    int capacity = 50;
    
    double max_value = fractionalKnapsack(values, weights, capacity);
    cout << "Maximum value: " << max_value << endl;
    
    return 0;
}
```

**Output** for the example:
```
Item 1: Take all (10 units)
Item 2: Take all (20 units)
Item 3: Take 66.6667% (20 units)
Maximum value: 240
```

## Algorithm Visualization
Below is a diagram illustrating the greedy selection process for items with values `[60, 100, 120]` and weights `[10, 20, 30]` with a knapsack capacity of 50:

```mermaid
gantt
    title Fractional Knapsack Example (Capacity = 50)
    dateFormat X
    axisFormat %X

    section Items (Value/Weight Ratio)
    Item 1 (6.0) :done, i1, 0, 10
    Item 2 (5.0) :done, i2, 10, 30
    Item 3 (4.0) :done, i3, 30, 60

    section Selected
    Item 1 (6.0) :active, s1, 0, 10
    Item 2 (5.0) :active, s2, 10, 30
    Item 3 (4.0) :active, s3, 30, 50
```

The diagram shows the items sorted by value-to-weight ratio and the selected portions (Item 1: full, Item 2: full, Item 3: 2/3).

## Time and Space Complexity
- **Time Complexity**:
  - Sorting items by value-to-weight ratio: `O(n log n)`.
  - Processing items: `O(n)`.
  - Total: `O(n log n)`.
- **Space Complexity**:
  - Storing items: `O(n)`.
  - Additional space for output: `O(1)` (excluding input storage).
  - Sorting may require `O(log n)` or `O(n)` depending on the algorithm used.

## Applications
- **Task Scheduling**: Allocating limited resources to tasks.
- **Investment**: Distributing funds across projects to maximize returns.
- **Networking**: Bandwidth allocation in communication systems.
- **Logistics**: Optimizing cargo loading in transportation.

## Strengths and Limitations
- **Strengths**:
  - Simple and efficient with `O(n log n)` time complexity.
  - Guarantees an optimal solution for the Fractional Knapsack Problem.
- **Limitations**:
  - Requires initial sorting.
  - Not applicable to the 0/1 Knapsack Problem, where items cannot be divided.
  - May not handle additional constraints (e.g., priorities or dependencies).

## Conclusion
The Fractional Knapsack Problem is efficiently solved using a greedy algorithm by prioritizing items with the highest value-to-weight ratio. With a time complexity of `O(n log n)`, it is ideal for scenarios where items can be taken fractionally, such as resource allocation or logistics optimization. For cases with indivisible items or additional constraints, alternative approaches like dynamic programming are required.

---
**References**:  
- Group 2 Presentation, 6 May 2025.