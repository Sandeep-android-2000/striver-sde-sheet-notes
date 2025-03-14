# Topological Sorting (DFS using Stack)

## Problem Statement

Given an adjacency list for a Directed Acyclic Graph (DAG) where `adj[u]` contains a list of all vertices `v` such that there exists a directed edge `u -> v`. Return the topological sort for the given graph.

Topological sorting for Directed Acyclic Graph (DAG) is a linear ordering of vertices such that for every directed edge `u -> v`, vertex `u` comes before `v` in the ordering.

**Note:** As there are multiple valid topological orders, you may return any of them. If your returned topological sort is correct, the output will be `1`, else `0`.

## Examples

### Example 1

**Input:**  
adj = `[[], [0], [0], [0]]`  

**Output:**  
`1`  

**Explanation:**  
A few valid topological orders for the given graph are:  
- `[3, 2, 1, 0]`  
- `[1, 2, 3, 0]`  
- `[2, 3, 1, 0]`  

### Example 2

**Input:**  
adj = `[[], [3], [3], [], [0,1], [0,2]]`  

**Output:**  
`1`  

**Explanation:**  
A few valid topological orders for the given graph are:  
- `[4, 5, 0, 1, 2, 3]`  
- `[5, 2, 4, 0, 1, 3]`  

## Constraints

- `2 ≤ V ≤ 1000`  
- `1 ≤ E ≤ (V * (V - 1)) / 2`  

---

## Approach (DFS using Stack)

1. Create a `visited` array initialized to `false` for all nodes.
2. Use a `stack` to store the topological order.
3. Perform a **DFS traversal** for each unvisited node:
   - Mark the node as visited.
   - Recursively visit all its unvisited neighbors.
   - After visiting all neighbors, push the current node onto the stack.
4. After DFS completes for all nodes, pop elements from the stack to get the topological order.

**Time Complexity:** O(V + E)  
**Space Complexity:** O(V) (for the stack and visited array)

---

## Code (C++ Implementation)

```cpp
class Solution {
private:
    void dfs(int node, vector<bool>& visited, stack<int>& st, vector<vector<int>>& adj) {
        visited[node] = true;
        
        for (auto neighbor : adj[node]) {
            if (!visited[neighbor]) {
                dfs(neighbor, visited, st, adj);
            }
        }
        
        // Push current node to stack after exploring all its neighbors
        st.push(node);
    }

public:
    vector<int> topologicalSort(vector<vector<int>>& adj) {
        int V = adj.size();
        vector<bool> visited(V, false);
        stack<int> st;
        vector<int> result;

        // Perform DFS for each unvisited node
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                dfs(i, visited, st, adj);
            }
        }

        // Pop elements from stack to get topological order
        while (!st.empty()) {
            result.push_back(st.top());
            st.pop();
        }

        return result;
    }
};
```

## Summary

- We used **DFS** and a **stack** to achieve topological sorting.
- Nodes are added to the stack after all their dependencies have been visited.
- The final order is obtained by popping elements from the stack.

