# Detect Cycle in a Directed Graph

## Problem Statement
Given a directed graph with `V` vertices (numbered from `0` to `V-1`) and `E` edges, check whether it contains any cycle or not. The graph is represented as an adjacency list, where `adj[i]` contains a list of vertices that are directly reachable from vertex `i`. Specifically, `adj[i][j]` represents an edge from vertex `i` to vertex `j`.

## Example
### Example 1:
#### Input:
```
V = 4, E = 4
Edges: [[0, 1], [1, 2], [2, 3], [3, 1]]
```
#### Output:
```
1
```
#### Explanation:
A cycle exists: `1 -> 2 -> 3 -> 1`

### Example 2:
#### Input:
```
V = 4, E = 3
Edges: [[0, 1], [1, 2], [2, 3]]
```
#### Output:
```
0
```
#### Explanation:
No cycle exists in the graph.

## Approach
To detect a cycle in a **directed graph**, we use **Depth-First Search (DFS)** and track visited nodes in two ways:
1. **vis[]**: A boolean array that keeps track of whether a node has been visited.
2. **pathVis[]**: An array that keeps track of nodes in the current DFS recursion path.

### Steps:
1. Initialize two arrays: `vis[]` (false initially) and `pathVis[]` (0 initially).
2. Perform DFS traversal for each unvisited node.
3. Mark the node as visited (`vis[node] = true`) and as part of the current recursion path (`pathVis[node] = 1`).
4. Traverse all its adjacent nodes:
   - If an adjacent node is **not visited**, perform DFS recursively.
   - If an adjacent node is **already in the current recursion path** (`pathVis[neighbor] == 1`), a cycle is detected.
5. After backtracking, reset `pathVis[node] = 0`.
6. If a cycle is found, return `true`, otherwise return `false` after checking all nodes.

## Code
```cpp
class Solution {
private:
    bool doesContainCycle(int node, vector<bool> &vis, vector<vector<int>> &adj, vector<int> &pathVis) {
        vis[node] = true;
        pathVis[node] = 1;  // Mark the node as visited in the current path

        for (auto neighbor : adj[node]) {
            if (!vis[neighbor]) {
                if (doesContainCycle(neighbor, vis, adj, pathVis)) 
                    return true;
            } else if (pathVis[neighbor] == 1) {  // A cycle is detected
                return true;
            }
        }

        pathVis[node] = 0; // Backtracking step
        return false;
    }

public:
    // Function to detect cycle in a directed graph.
    bool isCyclic(vector<vector<int>> &adj) {
        int n = adj.size();
        vector<bool> vis(n, false);
        vector<int> pathVis(n, 0);

        for (int i = 0; i < n; i++) {
            if (!vis[i]) {
                if (doesContainCycle(i, vis, adj, pathVis)) {
                    return true;
                }
            }
        }
        return false;
    }
};
```

## Time Complexity Analysis
- Each node is visited **once** in the DFS traversal → **O(V)**
- Each edge is processed **once** in the adjacency list traversal → **O(E)**
- **Total Complexity:** **O(V + E)**
