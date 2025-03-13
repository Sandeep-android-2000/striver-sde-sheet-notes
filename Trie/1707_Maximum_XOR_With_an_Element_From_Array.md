# 1707. Maximum XOR With an Element From Array

## Problem Statement
You are given an array `nums` consisting of non-negative integers. You are also given a `queries` array, where `queries[i] = [xi, mi]`.

The answer to the `i-th` query is the maximum bitwise XOR value of `xi` and any element of `nums` that does not exceed `mi`. In other words, the answer is:

\[ \max ( \text{nums}[j] \oplus x_i) \]  for all `j` such that `nums[j] <= mi`. If all elements in `nums` are larger than `mi`, then the answer is `-1`.

Return an integer array `answer` where `answer.length == queries.length` and `answer[i]` is the answer to the `i-th` query.

### Example 1:
**Input:**
```cpp
nums = [0,1,2,3,4], queries = [[3,1],[1,3],[5,6]]
```
**Output:**
```cpp
[3,3,7]
```
**Explanation:**
- For query `[3,1]`: Elements `≤ 1` are `{0,1}`. The best XOR values are `0^3 = 3` and `1^3 = 2`. Maximum is `3`.
- For query `[1,3]`: Elements `≤ 3` are `{0,1,2,3}`. The best XOR value is `1^2 = 3`.
- For query `[5,6]`: Elements `≤ 6` are `{0,1,2,3,4}`. The best XOR value is `5^2 = 7`.

### Example 2:
**Input:**
```cpp
nums = [5,2,4,6,6,3], queries = [[12,4],[8,1],[6,3]]
```
**Output:**
```cpp
[15,-1,5]
```

---

## Approach

### Brute Force Approach (TLE)
**Time Complexity:** \(O(Q \times N)\)  
For each query `(xi, mi)`, we iterate through all elements in `nums` and find the best possible XOR result where `nums[j] <= mi`. This results in a time complexity of \(O(Q \times N)\), which is too slow for large inputs.

### Better Approach (Sorting + Binary Search) - O(N logN + Q logN)
1. **Sort** the `nums` array.
2. **Sort** the queries based on `mi`.
3. Iterate through queries:
   - Consider only elements `≤ mi` using a sorted structure.
   - Use **Binary Search** to find elements `≤ mi`.
   - Iterate through valid elements and compute the best XOR.

#### Implementation in C++
```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> maximizeXor(vector<int>& nums, vector<vector<int>>& queries) {
    sort(nums.begin(), nums.end());
    vector<int> ans;
    
    for (auto& q : queries) {
        int xi = q[0], mi = q[1], maxXor = -1;
        
        auto it = upper_bound(nums.begin(), nums.end(), mi);
        for (auto jt = nums.begin(); jt != it; jt++) {
            maxXor = max(maxXor, *jt ^ xi);
        }
        
        ans.push_back(maxXor);
    }
    
    return ans;
}

int main() {
    vector<int> nums = {5,2,4,6,6,3};
    vector<vector<int>> queries = {{12,4}, {8,1}, {6,3}};
    vector<int> result = maximizeXor(nums, queries);
    for (int x : result) cout << x << " ";
    return 0;
}
```

### Optimal Approach (Trie + Sorting) - O((N + Q) logN)
### **Algorithm**
1. **Sort** `nums` and queries by `mi`.
2. Insert numbers into a **Trie** while processing queries.
3. Use Trie to compute the maximum XOR efficiently.
4. Store results in the original query order.

### **Trie Node Structure**
A Trie stores binary representations of numbers (up to 30 bits, since `≤ 10^9`). Each node has `0` and `1` children.

### **Implementation in C++**
```cpp
#include <bits/stdc++.h>
using namespace std;

struct TrieNode {
    TrieNode* children[2];
    TrieNode() {
        children[0] = children[1] = nullptr;
    }
};

class Trie {
private:
    TrieNode* root;
public:
    Trie() { root = new TrieNode(); }
    
    void insert(int num) {
        TrieNode* node = root;
        for (int i = 30; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (!node->children[bit]) node->children[bit] = new TrieNode();
            node = node->children[bit];
        }
    }
    
    int getMaxXOR(int x) {
        TrieNode* node = root;
        int maxXor = 0;
        for (int i = 30; i >= 0; i--) {
            int bit = (x >> i) & 1;
            if (node->children[1 - bit]) {
                maxXor |= (1 << i);
                node = node->children[1 - bit];
            } else {
                node = node->children[bit];
            }
        }
        return maxXor;
    }
};

vector<int> maximizeXor(vector<int>& nums, vector<vector<int>>& queries) {
    sort(nums.begin(), nums.end());
    vector<pair<int, pair<int, int>>> q;
    vector<int> ans(queries.size());
    
    for (int i = 0; i < queries.size(); i++) {
        q.push_back({queries[i][1], {queries[i][0], i}});
    }
    sort(q.begin(), q.end());
    
    Trie trie;
    int idx = 0;
    for (auto [mi, xi_i] : q) {
        auto [xi, i] = xi_i;
        while (idx < nums.size() && nums[idx] <= mi) {
            trie.insert(nums[idx]);
            idx++;
        }
        ans[i] = (idx == 0 ? -1 : trie.getMaxXOR(xi));
    }
    
    return ans;
}
```
