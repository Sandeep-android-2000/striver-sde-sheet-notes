# Cycle Detection in an Undirected Graph

## Problem Statement
Given an undirected graph with `V` vertices labeled from `0` to `V-1` and `E` edges, check whether the graph contains any cycle or not. The graph is represented as an adjacency list, where `adj[i]` contains all the vertices directly connected to vertex `i`.

### **Note**
- The adjacency list represents undirected edges, meaning that if there is an edge between vertex `i` and vertex `j`, both `j` will be in `adj[i]` and `i` will be in `adj[j]`.

## Examples

### **Example 1**
#### **Input:**
```cpp
adj = [[1], [0,2,4], [1,3], [2,4], [1,3]]
```
#### **Output:**
```cpp
1
```
#### **Explanation:**
The cycle exists: `1 -> 2 -> 3 -> 4 -> 1`

---

### **Example 2**
#### **Input:**
```cpp
adj = [[], [2], [1,3], [2]]
```
#### **Output:**
```cpp
0
```
#### **Explanation:**
No cycle is present in the graph.

## Approach
The problem can be solved using **DFS traversal** while keeping track of the parent node to ensure that we do not mistakenly detect a cycle due to bidirectional edges in an undirected graph.

### **Steps:**
1. **Initialize a `visited` array** to keep track of visited nodes.
2. **Perform DFS for each component** of the graph (handling disconnected graphs).
3. **Maintain a `parent` variable** while performing DFS.
4. **Detect a cycle:**
   - If a visited neighbor is **not the parent**, a cycle is found.
   - Otherwise, continue traversal.

## Code Implementation
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
    private:
    bool dfs(int node, int parent, vector<bool>& vis, vector<int> adj[]) { 
        vis[node] = true;
        
        for (auto adjNode : adj[node]) {
            if (!vis[adjNode]) {
                if (dfs(adjNode, node, vis, adj)) 
                    return true;
            } 
            else if (adjNode != parent)
                return true;
        }
        
        return false;
    }
    
    public:
    // Function to detect cycle in an undirected graph.
    bool isCycle(int V, vector<int> adj[]) {
        vector<bool> vis(V, false);
        for (int i = 0; i < V; i++) {
            if (!vis[i]) {
                if (dfs(i, -1, vis, adj)) 
                    return true;
            }
        }
        return false;
    }
};
```

## Time Complexity Analysis
- Each **vertex** is visited once → **O(V)**
- Each **edge** is traversed twice (once from `u -> v` and once from `v -> u`) → **O(2E)**
- **Overall Time Complexity:** **O(V + E)** (Optimal for graph traversal)
