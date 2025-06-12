---
title: "Activity Selection Problem"
date: 2025-05-06 00:00:00 +0800
categories: [Algorithms, Greedy]
tags: [Activity Selection, Greedy Algorithm, Optimization]
---

# Activity Selection Problem

The **Activity Selection Problem** is a classic optimization problem in computer science aimed at selecting the maximum number of non-overlapping activities. Given a set of activities with start and finish times, the goal is to choose the largest subset of compatible activities, where two activities are compatible if one finishes before the other starts.

## Problem Overview
- **Input**: A set of activities with start times (`s`) and finish times (`f`).
- **Objective**: Maximize the number of activities that can be performed by a single resource without overlapping.
- **Key Idea**: Use a **greedy algorithm** by selecting the activity with the earliest finish time at each step to maximize the remaining time for other activities.

## Greedy Algorithm
1. Sort all activities by their finish times.
2. Select the first activity (earliest finish time).
3. For each subsequent activity, select it if its start time is greater than or equal to the finish time of the last selected activity.

This approach ensures an optimal solution by maximizing the number of non-overlapping activities.

### Pseudocode
```
ACTIVITY-SELECTOR(s, f, n)
    A = {a1} // Select the first activity
    j = 1    // Index of the last selected activity
    for i = 2 to n
        if s[i] >= f[j] // If start time is after or at the last finish time
            A = A ∪ {ai} // Add activity ai to the set
            j = i       // Update the last selected activity
    return A
```

## C++ Implementation
Below is a C++ implementation of the Activity Selection Problem:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Structure to represent an activity
struct Activity {
    int start, finish;
};

// Comparator to sort activities by finish time
bool compare(Activity a, Activity b) {
    return a.finish < b.finish;
}

// Function to select maximum non-overlapping activities
void activitySelection(vector<int> s, vector<int> f, int n) {
    vector<Activity> activities(n);
    for (int i = 0; i < n; i++) {
        activities[i] = {s[i], f[i]};
    }
    
    // Sort activities by finish time
    sort(activities.begin(), activities.end(), compare);
    
    // Select the first activity
    cout << "Selected activities (index): 0 ";
    int j = 0; // Index of last selected activity
    
    // Check each activity
    for (int i = 1; i < n; i++) {
        if (activities[i].start >= activities[j].finish) {
            cout << i << " ";
            j = i;
        }
    }
    cout << endl;
}

int main() {
    // Example: Start and finish times
    vector<int> start = {1, 3, 0, 5, 8, 5};
    vector<int> finish = {2, 4, 6, 7, 9, 9};
    int n = start.size();
    
    activitySelection(start, finish, n);
    return 0;
}
```

**Output** for the example:
```
Selected activities (index): 0 1 3 4
```

## Algorithm Visualization
Below is a diagram illustrating the greedy selection process for activities with start times `[1, 3, 0, 5, 8, 5]` and finish times `[2, 4, 6, 7, 9, 9]`:

```mermaid
gantt
    title Activity Selection Example
    dateFormat HH
    axisFormat %H

    section Activities
    Activity 1 :done, a1, 01, 02
    Activity 2 :done, a2, 03, 04
    Activity 3 :done, a3, 00, 06
    Activity 4 :done, a4, 05, 07
    Activity 5 :done, a5, 08, 09
    Activity 6 :done, a6, 05, 09

    section Selected
    Activity 1 :active, s1, 01, 02
    Activity 2 :active, s2, 03, 04
    Activity 4 :active, s4, 05, 07
    Activity 5 :active, s5, 08, 09
```

The diagram shows all activities and highlights the selected ones (1, 2, 4, 5) based on the greedy algorithm.

## Time and Space Complexity
- **Time Complexity**: 
  - Sorting activities by finish time: `O(n log n)`.
  - Selecting activities: `O(n)`.
  - Total: `O(n log n)`.
- **Space Complexity**: 
  - Storing activities: `O(n)`.
  - Additional space for output: `O(n)` in the worst case.
  - Sorting may require `O(log n)` or `O(n)` depending on the algorithm used.

## Applications
- **Scheduling & Facilities**: Classroom, meeting, lab, or sports scheduling.
- **Operating Systems**: CPU process scheduling, memory allocation.
- **Logistics**: Delivery schedules, vehicle routing.
- **Telecommunications**: Bandwidth allocation, data transmission scheduling.

## Strengths and Limitations
- **Strengths**:
  - Simple and efficient for large datasets.
  - Guarantees an optimal solution for the basic problem.
- **Limitations**:
  - Requires initial sorting.
  - Not suitable for problems with additional constraints (e.g., priorities, distances, or costs).

## Conclusion
The Activity Selection Problem is efficiently solved using a greedy algorithm that sorts activities by finish time, achieving an optimal solution with `O(n log n)` time complexity. It is ideal for scheduling tasks but may require advanced methods like dynamic programming or metaheuristics for more complex scenarios.

---
**References**:  
- Group 1 Presentation, 6 May 2025.