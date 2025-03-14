# Number of Islands (Leetcode 200)

## Problem Statement
Given an `m x n` 2D binary grid `grid` which represents a map of '1's (land) and '0's (water), return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands **horizontally or vertically**. You may assume all four edges of the grid are surrounded by water.

## Examples

### **Example 1**
#### **Input:**
```cpp
grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
```
#### **Output:**
```cpp
1
```
#### **Explanation:**
There is only one large connected island.

---

### **Example 2**
#### **Input:**
```cpp
grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
```
#### **Output:**
```cpp
3
```
#### **Explanation:**
There are three separate islands in the grid.

## Constraints
- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

## Approach
This problem can be efficiently solved using **BFS traversal**. The idea is to traverse the grid and initiate a BFS whenever we encounter an unvisited land (`'1'`). The BFS will mark all connected land as visited, ensuring that each island is counted only once.

### **Steps:**
1. **Initialize a `visited` matrix** to track visited cells.
2. **Iterate through the grid** and start BFS for each unvisited `'1'`.
3. **Use BFS traversal** to explore all connected land cells:
   - Use a queue to process cells in a breadth-first manner.
   - Store directional movements `[(-1,0), (0,1), (1,0), (0,-1)]` to check all 4 possible directions.
   - Continue expanding until all parts of the island are visited.
4. **Count the number of BFS calls**, as each represents a distinct island.

## Code Implementation
```cpp
#include <bits/stdc++.h>
using namespace std;

class Solution {
    bool isValid(int x, int y, int m, int n) {
        return (x >= 0 && x < m && y >= 0 && y < n);
    }

    int bfs(int i, int j, vector<vector<bool>>& vis, vector<vector<char>>& grid) {
        vis[i][j] = true;
        queue<pair<int, int>> q;
        int m = grid.size(), n = grid[0].size();

        q.push({i, j});
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, 1, 0, -1};

        while (!q.empty()) {
            auto it = q.front();
            int x = it.first;
            int y = it.second;

            q.pop();

            for (int i = 0; i < 4; i++) {
                int newX = x + dx[i];
                int newY = y + dy[i];

                if (isValid(newX, newY, m, n) && !vis[newX][newY] && grid[newX][newY] == '1') {
                    vis[newX][newY] = true;
                    q.push({newX, newY});
                }
            }
        }

        return 1;
    }

public:
    int numIslands(vector<vector<char>>& grid) {
        int count = 0;
        int m = grid.size(), n = grid[0].size();
        vector<vector<bool>> vis(m, vector<bool>(n, false));

        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (!vis[i][j] && grid[i][j] == '1') {
                    count += bfs(i, j, vis, grid);
                }
            }
        }

        return count;
    }
};
```

## Time Complexity Analysis
- **Each cell is visited once:** **O(m * n)**
- **Each edge is processed at most once:** **O(4 * m * n)**
- **Overall Time Complexity:** **O(m * n)** (Optimal for grid traversal)
