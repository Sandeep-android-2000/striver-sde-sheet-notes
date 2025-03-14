# Topological Sorting of a Directed Acyclic Graph (DAG)

## Problem Statement
Given an adjacency list for a **Directed Acyclic Graph (DAG)** where `adj[u]` contains a list of all vertices `v` such that there exists a directed edge `u -> v`, return a **topological sort** for the given graph.

Topological sorting for a DAG is a **linear ordering** of vertices such that for every directed edge `u -> v`, vertex `u` comes before `v` in the ordering.

**Note:** Since multiple topological orders are possible, you may return any of them. If your returned order is a valid topological sort, then the output will be **1**, otherwise **0**.

## Examples

### Example 1:
#### **Input:**  
```
adj = [[], [0], [0], [0]]
```
#### **Output:**  
```
1
```
#### **Explanation:**  
The output `1` denotes that the order is valid. Few valid Topological orders for the given graph are:  
- `[3, 2, 1, 0]`
- `[1, 2, 3, 0]`
- `[2, 3, 1, 0]`

---

### Example 2:
#### **Input:**  
```
adj = [[], [3], [3], [], [0,1], [0,2]]
```
#### **Output:**  
```
1
```
#### **Explanation:**  
The output `1` denotes that the order is valid. Few valid Topological orders for the graph are:  
- `[4, 5, 0, 1, 2, 3]`
- `[5, 2, 4, 0, 1, 3]`

---

## Constraints
- \( 2 \leq V \leq 1000 \)  
- \( 1 \leq E \leq rac{V 	imes (V - 1)}{2} \)

---

## Approach
We can perform **topological sorting** using **Kahn's Algorithm (BFS)**:

1. **Compute the in-degree of each node** (number of incoming edges).
2. **Add all nodes with in-degree `0` to a queue**.
3. **Process nodes in the queue**:
   - Remove the front node.
   - Append it to the result.
   - Reduce the in-degree of its adjacent nodes.
   - If an adjacent node’s in-degree becomes `0`, push it to the queue.
4. **If all nodes are processed, the ordering is valid**.

This ensures that nodes appear before their dependencies.

---

## Code (C++)
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
public:
    vector<int> topologicalSort(vector<vector<int>>& adj) {
        int n = adj.size();
        vector<int> indegree(n, 0);
        
        // Compute in-degree for each node
        for (int i = 0; i < n; i++) {
            for (auto neighbor : adj[i]) {
                indegree[neighbor]++;
            }
        }
        
        queue<int> q;
        
        // Push nodes with in-degree 0
        for (int i = 0; i < n; i++) {
            if (indegree[i] == 0) {
                q.push(i);
            }
        }

        vector<int> result;
        
        while (!q.empty()) {
            int node = q.front();
            q.pop();
            result.push_back(node);
            
            for (auto neighbor : adj[node]) {
                indegree[neighbor]--;
                if (indegree[neighbor] == 0) {
                    q.push(neighbor);
                }
            }
        }

        return result;
    }
};
```

---

## Time Complexity Analysis
- **Computing in-degrees:** \( O(V + E) \)
- **Processing queue:** \( O(V + E) \) (each node and edge are processed once)
- **Overall Complexity:** **\( O(V + E) \)**, which is optimal for DAGs.

---

## Space Complexity
- **In-degree array:** \( O(V) \)
- **Queue storage:** \( O(V) \)
- **Adjacency list storage:** \( O(V + E) \)  
- **Overall Space Complexity:** **\( O(V + E) \)**

---

## Summary
- We used **Kahn's Algorithm (BFS)** to implement topological sorting.
- The algorithm ensures that all dependencies are resolved before processing a node.
- The solution runs in **\( O(V + E) \)** time complexity, making it efficient for large graphs.

---