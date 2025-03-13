# Clone Graph (Leetcode 133)

## Problem Statement
Given a reference of a node in a **connected undirected graph**, return a **deep copy (clone) of the graph**.

Each node contains a value (`int`) and a list (`List[Node]`) of its neighbors.

### **Example 1:**
#### **Input:**
```
adjList = [[2,4],[1,3],[2,4],[1,3]]
```
#### **Output:**
```
[[2,4],[1,3],[2,4],[1,3]]
```
#### **Explanation:**
- The graph consists of 4 nodes:
  - Node `1` is connected to nodes `2` and `4`.
  - Node `2` is connected to nodes `1` and `3`.
  - Node `3` is connected to nodes `2` and `4`.
  - Node `4` is connected to nodes `1` and `3`.

### **Example 2:**
#### **Input:**
```
adjList = [[]]
```
#### **Output:**
```
[[]]
```
#### **Explanation:**
- The graph contains only one node with `val = 1`, and it has no neighbors.

### **Example 3:**
#### **Input:**
```
adjList = []
```
#### **Output:**
```
[]
```
#### **Explanation:**
- The graph is empty and does not contain any nodes.

## **Constraints:**
- The number of nodes in the graph is in the range **[0, 100]**.
- `1 <= Node.val <= 100`
- `Node.val` is unique for each node.
- There are no repeated edges and no self-loops.
- The graph is **connected** and all nodes can be visited starting from the given node.

---

## **Approach**
### **Understanding the Problem**
We need to create a **deep copy** of the given graph while preserving the structure and connections between nodes.

### **Key Observations**
- Each node has a unique value.
- The graph is connected, meaning we can reach all nodes from `node 1`.
- We need to avoid cloning the same node multiple times.

### **Steps to Solve**
1. **Handle the base case**: If `node == nullptr`, return `nullptr` (empty graph).
2. **Use BFS traversal** to clone nodes:
   - Maintain a mapping (`unordered_map<Node*, Node*> copies`) from original nodes to their cloned counterparts.
   - Use a queue (`queue<Node*> q`) for BFS traversal.
   - For each node, clone its neighbors if they haven't been cloned yet.
   - Connect the cloned nodes accordingly.
3. **Return the cloned version** of the first node.

---

## **Code Implementation (C++)**
```cpp
#include <unordered_map>
#include <queue>

class Node {
public:
    int val;
    std::vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = std::vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = std::vector<Node*>();
    }
    Node(int _val, std::vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};

class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (!node) return nullptr; // Handle empty graph case
        
        std::unordered_map<Node*, Node*> copies; // Map original -> cloned node
        std::queue<Node*> q;
        
        // Clone the first node and push it into the queue
        Node* clonedNode = new Node(node->val);
        copies[node] = clonedNode;
        q.push(node);
        
        while (!q.empty()) {
            Node* curr = q.front();
            q.pop();
            
            // Clone neighbors
            for (Node* neighbor : curr->neighbors) {
                if (copies.find(neighbor) == copies.end()) { // If neighbor isn't cloned yet
                    copies[neighbor] = new Node(neighbor->val);
                    q.push(neighbor);
                }
                // Link the cloned node to the cloned neighbor
                copies[curr]->neighbors.push_back(copies[neighbor]);
            }
        }
        
        return copies[node]; // Return the cloned first node
    }
};
```

---

## **Complexity Analysis**
### **Time Complexity:**
- **O(N + E)**: We visit each **node (N)** and each **edge (E)** once.

### **Space Complexity:**
- **O(N)**: We use extra space for:
  - The `unordered_map<Node*, Node*> copies` (stores all nodes).
  - The queue (`queue<Node*> q`), which in the worst case holds all nodes.

