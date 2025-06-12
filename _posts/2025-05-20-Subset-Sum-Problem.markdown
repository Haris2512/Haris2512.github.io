---
title: "Subset Sum Problem"
date: 2025-05-20 00:00:00 +0800
categories: [Algorithms, Dynamic Programming]
tags: [Subset Sum, NP-Complete, Dynamic Programming]
---

# Subset Sum Problem

The **Subset Sum Problem (SSP)** is a classic NP-Complete problem in computer science. Given a set of integers and a target sum, the task is to determine whether there exists a subset of the set whose elements sum exactly to the target. This decision problem has significant applications in cryptography, resource allocation, and artificial intelligence.

## Problem Overview
- **Input**: A set of integers (e.g., `{3, 34, 4, 12, 5, 2}`) and a target sum (e.g., `9`).
- **Objective**: Determine if there exists a subset whose sum equals the target (e.g., `{4, 5}` sums to `9`).
- **Type**: Decision problem (returns "yes" or "no").
- **Applications**:
  - Cryptography (e.g., knapsack-based encryption).
  - Resource allocation (e.g., budgeting, project selection).
  - Bioinformatics (e.g., analyzing gene expression).
  - Finance (e.g., selecting assets to meet investment targets).
  - Game design (e.g., puzzles requiring specific score combinations).

## Variations
1. **Bounded Subset Sum**: Each element can be used at most once.
   - Example: Set `{3, 34, 4, 12, 5, 2}`, Target `9` → Subset `{4, 5}`.
2. **Unbounded Subset Sum**: Elements can be used multiple times.
   - Example: Set `{1, 3, 4}`, Target `6` → Subsets like `{3, 3}` or `{1, 1, 4}`.
3. **Partition Problem**: Divide the set into two subsets with equal sums.
   - Example: Set `{1, 5, 11, 5}`, Target `11` → Subsets `{11}` and `{1, 5, 5}`.
4. **Exact k Elements Subset Sum**: Subset must have exactly `k` elements.
   - Example: Set `{1, 2, 3, 4, 5}`, Target `9`, `k=2` → Subset `{4, 5}`.
5. **Multi-Target Subset Sum**: Satisfy multiple criteria (e.g., sum and weight).
   - Example: Items `{A: (40, 1kg), B: (60, 1.5kg), C: (30, 0.5kg)}`, Target: sum ≤ `100`, weight ≤ `2kg` → `{A, C}`.
6. **Approximate Subset Sum**: Find a subset closest to the target without exceeding it.
   - Example: Set `{2, 5, 10, 14}`, Target `16` → Subset `{10, 5}` (sum `15`).

## Solution Approaches
### Recursive Approach
- **Method**: Exhaustively check all subsets by including or excluding each element.
- **Pseudocode**:
  ```
  SUBSET-SUM(arr, n, sum)
      if sum == 0
          return True
      if n == 0 and sum != 0
          return False
      return SUBSET-SUM(arr, n-1, sum) or SUBSET-SUM(arr, n-1, sum - arr[n-1])
  ```
- **Complexity**: 
  - Time: `O(2^n)` (exponential, inefficient for large inputs).
  - Space: `O(n)` (recursion stack).

### Dynamic Programming (DP) Approach
#### Memoization (Top-Down)
- Uses a table `dp[n][sum]` to cache results of subproblems.
- **Complexity**:
  - Time: `O(n × sum)`.
  - Space: `O(n × sum)`.

#### Tabulation (Bottom-Up)
- Builds a table `dp[n+1][sum+1]` iteratively.
- `dp[i][j] = True` if a subset of `arr[0..i-1]` sums to `j`.
- **Initialization**:
  - `dp[i][0] = True` (empty subset sums to 0).
  - `dp[0][j] = False` for `j ≠ 0` (no elements cannot sum to non-zero).
- **Recurrence**:
  - If `j < arr[i-1]`: `dp[i][j] = dp[i-1][j]`.
  - Else: `dp[i][j] = dp[i-1][j] or dp[i-1][j - arr[i-1]]`.
- **Complexity**:
  - Time: `O(n × sum)`.
  - Space: `O(n × sum)`.

#### Space-Optimized Tabulation
- Uses two 1D arrays (`prev` and `curr`) to reduce space to `O(sum)`.
- **Recurrence**:
  - If `j < arr[i]`: `curr[j] = prev[j]`.
  - Else: `curr[j] = prev[j] or prev[j - arr[i]]`.
- **Complexity**:
  - Time: `O(n × sum)`.
  - Space: `O(sum)`.

## C++ Implementation (Space-Optimized DP)
Below is a C++ implementation using space-optimized dynamic programming:

```cpp
#include <iostream>
#include <vector>

using namespace std;

// Function to check if a subset sums to target
bool subsetSum(vector<int>& arr, int target) {
    int n = arr.size();
    vector<bool> prev(target + 1, false);
    vector<bool> curr(target + 1, false);
    
    // Base case: sum = 0 is always possible with empty subset
    prev[0] = true;
    
    // Iterate through each element
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= target; j++) {
            if (j < arr[i-1]) {
                curr[j] = prev[j]; // Cannot include current element
            } else {
                curr[j] = prev[j] || prev[j - arr[i-1]]; // Include or exclude
            }
        }
        prev = curr; // Update prev for next iteration
    }
    
    return prev[target];
}

// Function to find and print one valid subset (if exists)
void printSubset(vector<int>& arr, int target) {
    int n = arr.size();
    vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));
    dp[0][0] = true;
    
    // Build DP table
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= target; j++) {
            if (j < arr[i-1]) {
                dp[i][j] = dp[i-1][j];
            } else {
                dp[i][j] = dp[i-1][j] || dp[i-1][j - arr[i-1]];
            }
        }
    }
    
    // If no subset exists
    if (!dp[n][target]) {
        cout << "No subset found with sum " << target << endl;
        return;
    }
    
    // Backtrack to find subset
    vector<int> subset;
    int i = n, j = target;
    while (i > 0 && j > 0) {
        if (dp[i][j] && !dp[i-1][j]) {
            subset.push_back(arr[i-1]);
            j -= arr[i-1];
        }
        i--;
    }
    
    cout << "Subset with sum " << target << ": ";
    for (int x : subset) cout << x << " ";
    cout << endl;
}

int main() {
    vector<int> arr = {3, 34, 4, 12, 5, 2};
    int target = 9;
    
    if (subsetSum(arr, target)) {
        cout << "Subset exists for target sum " << target << endl;
        printSubset(arr, target);
    } else {
        cout << "No subset exists for target sum " << target << endl;
    }
    
    return 0;
}
```

**Output** for the example:
```
Subset exists for target sum 9
Subset with sum 9: 4 5
```

## Algorithm Visualization
Below is a diagram illustrating the DP table for the example `arr = {3, 34, 4, 12, 5, 2}`, `target = 9`:

```mermaid
graph TD
    A[DP Table: arr = {3, 34, 4, 12, 5, 2}, Target = 9] --> B[i=0: Empty]
    B --> |sum=0| C[True]
    B --> |sum=1..9| D[False]
    A --> E[i=1: {3}]
    E --> |sum=0| F[True]
    E --> |sum=3| G[True]
    E --> |sum=1,2,4..9| H[False]
    A --> I[i=2: {3, 34}]
    I --> |sum=0,3| J[True]
    I --> |sum=1,2,4..9| K[False]
    A --> L[i=3: {3, 34, 4}]
    L --> |sum=0,3,4,7| M[True]
    L --> |sum=1,2,5,6,8,9| N[False]
    A --> O[i=4: {3, 34, 4, 12}]
    O --> |sum=0,3,4,7| P[True]
    O --> |sum=1,2,5,6,8,9| Q[False]
    A --> R[i=5: {3, 34, 4, 12, 5}]
    R --> |sum=0,3,4,5,7,8,9| S[True]
    R --> |sum=1,2,6| T[False]
    A --> U[i=6: {3, 34, 4, 12, 5, 2}]
    U --> |sum=0,2,3,4,5,7,8,9| V[True]
    U --> |sum=1,6| W[False]
```

The table shows how `dp[i][j]` is filled, with `dp[6][9] = True` indicating a valid subset `{4, 5}`.

## Time and Space Complexity
- **Recursive Approach**:
  - Time: `O(2^n)`.
  - Space: `O(n)`.
- **DP (Memoization/Tabulation)**:
  - Time: `O(n × sum)`.
  - Space: `O(n × sum)`.
- **Space-Optimized DP**:
  - Time: `O(n × sum)`.
  - Space: `O(sum)`.

## Applications
- **Cryptography**: Basis for knapsack-based public-key systems.
- **Resource Allocation**: Selecting projects within budget constraints.
- **Bioinformatics**: Analyzing gene combinations for specific expression levels.
- **Finance**: Choosing asset combinations to meet investment goals.
- **Game Design**: Creating puzzles requiring specific score combinations.

## Strengths and Limitations
- **Strengths**:
  - DP provides efficient solutions for moderate input sizes.
  - Space-optimized DP reduces memory usage significantly.
  - Applicable to real-world problems like budgeting and logistics.
- **Limitations**:
  - NP-Complete, making large instances computationally expensive.
  - DP becomes impractical for very large `sum` values due to space requirements.
  - Recursive approach is infeasible for large `n`.

## Conclusion
The Subset Sum Problem is a versatile NP-Complete problem with applications in cryptography, finance, and resource allocation. While the recursive approach is simple but inefficient (`O(2^n)`), dynamic programming offers a practical solution with `O(n × sum)` time complexity, further optimized to `O(sum)` space. Its variations, like bounded, unbounded, and multi-target, extend its relevance to diverse real-world scenarios.

---
**References**:  
- Group 5 Presentation, 20 May 2025.