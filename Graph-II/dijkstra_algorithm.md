# Dijkstra Algorithm

## Problem Statement

Given a weighted, undirected, and connected graph where you have an adjacency list `adj`, find the shortest distance of all the vertices from the source vertex `src`, and return a list of integers denoting the shortest distance between each node and the source vertex `src`.

**Note:** The graph doesn't contain any negative weight edges.

## Examples

### Example 1
**Input:**
```
adj = [[[1, 9]], [[0, 9]]], src = 0
```
**Output:**
```
[0, 9]
```
**Explanation:**  
The source vertex is 0. Hence, the shortest distance of node 0 is 0, and the shortest distance from node 0 to 1 is 9.

### Example 2
**Input:**
```
adj = [[[1, 1], [2, 6]], [[2, 3], [0, 1]], [[1, 3], [0, 6]]], src = 2
```
**Output:**
```
[4, 3, 0]
```
**Explanation:**  
For nodes 2 to 0, we can follow the path 2 → 1 → 0. This has a distance of 1 + 3 = 4, whereas the direct path 2 → 0 has a distance of 6.  
So, the shortest path from 2 to 0 is 4. The shortest distance from 2 to 1 is 3.

## Constraints

- `1 ≤ no. of vertices ≤ 1000`
- `0 ≤ adj[i][j] ≤ 1000`
- `0 ≤ src < no. of vertices`

---

## Approach

Dijkstra’s Algorithm is used to find the shortest path from a single source node to all other nodes in a weighted graph. It works as follows:

1. **Initialize Distances:**  
   - Create a `dist` array initialized to `INT_MAX` to store the shortest distances from `src` to all vertices.
   - Set `dist[src] = 0` since the distance from the source to itself is 0.

2. **Min-Heap Priority Queue:**  
   - Use a min-heap (`priority_queue`) to store `{distance, node}` pairs.
   - The priority queue ensures that the node with the smallest known distance is processed first.

3. **Processing Nodes:**  
   - Extract the node with the smallest distance from the priority queue.
   - Update the distances of its adjacent nodes if a shorter path is found.

4. **Relaxation of Edges:**  
   - For each adjacent node, if `current_distance + edge_weight < dist[neighbor]`, update `dist[neighbor]` and push it into the priority queue.

5. **Final Distances:**  
   - Once all nodes are processed, `dist` contains the shortest distances from `src` to every other node.

---

## Implementation

```cpp
#define pi pair<int,int>
class Solution {
  public:
    // Function to find the shortest distance of all the vertices
    // from the source vertex src.
    vector<int> dijkstra(vector<vector<pair<int, int>>> &adj, int src) {
        int n = adj.size();
        vector<int> dist(n, INT_MAX);
        dist[src] = 0;

        priority_queue<pi, vector<pi>, greater<pi>> pq;
        pq.push({0, src});

        while (!pq.empty()) {
            auto it = pq.top();
            pq.pop();

            int node = it.second;
            int d = it.first;

            for (auto neighbor : adj[node]) {
                int edgeWeight = neighbor.second;
                int neighborNode = neighbor.first;

                if (d + edgeWeight < dist[neighborNode]) {
                    dist[neighborNode] = d + edgeWeight;
                    pq.push({dist[neighborNode], neighborNode});
                }
            }
        }

        return dist;
    }
};
```

## Time Complexity

- **O((V + E) log V)**  
  - Each node is inserted into the priority queue at most once (`O(V log V)`).
  - Each edge is processed once (`O(E log V)` due to heap operations).  
  - Thus, the overall time complexity is **O((V + E) log V)**.

## Space Complexity

- **O(V + E)**  
  - Storing the adjacency list takes **O(V + E)** space.
  - The priority queue takes **O(V)** space.
  - The `dist` array takes **O(V)** space.

---

## Summary

- Uses a **min-heap** to prioritize the next closest node.
- **Efficient for graphs with positive edge weights.**
- **Time complexity:** `O((V + E) log V)` using a priority queue.
