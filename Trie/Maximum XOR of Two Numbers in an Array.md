# Maximum XOR of Two Numbers in an Array (Trie Method) | C++ | Java

## Problem Statement
Given an array of integers, find the maximum XOR of any two numbers in the array.

## Approach Using Trie
Since XOR operates bitwise, we can use a **binary Trie** to efficiently determine the maximum XOR possible for each element in the array. The core idea is to:
1. **Insert** each number into a Trie (bitwise representation).
2. **Find** the number in the Trie that gives the maximum XOR with the current number.

### Trie Data Structure
Each node in the Trie has:
- Two children (0 and 1) representing bits.
- An insert function to add numbers bit by bit.
- A method to find the best possible XOR for a given number.

## C++ Implementation
```cpp
#include <bits/stdc++.h>
using namespace std;

class TrieNode {
public:
    TrieNode* child[2];
    TrieNode() {
        child[0] = child[1] = NULL;
    }
};

class Trie {
public:
    TrieNode* root;
    Trie() { root = new TrieNode(); }

    void insert(int num) {
        TrieNode* node = root;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (!node->child[bit])
                node->child[bit] = new TrieNode();
            node = node->child[bit];
        }
    }

    int findMaxXOR(int num) {
        TrieNode* node = root;
        int maxXOR = 0;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (node->child[1 - bit]) {  // Try opposite bit for max XOR
                maxXOR |= (1 << i);
                node = node->child[1 - bit];
            } else {
                node = node->child[bit];
            }
        }
        return maxXOR;
    }
};

int findMaximumXOR(vector<int>& nums) {
    Trie trie;
    for (int num : nums)
        trie.insert(num);
    
    int maxXOR = 0;
    for (int num : nums)
        maxXOR = max(maxXOR, trie.findMaxXOR(num));
    
    return maxXOR;
}

int main() {
    vector<int> nums = {3, 10, 5, 25, 2, 8};
    cout << "Maximum XOR: " << findMaximumXOR(nums) << endl;
    return 0;
}
```

## Java Implementation
```java
class TrieNode {
    TrieNode[] children = new TrieNode[2];
}

class Trie {
    TrieNode root = new TrieNode();

    void insert(int num) {
        TrieNode node = root;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (node.children[bit] == null)
                node.children[bit] = new TrieNode();
            node = node.children[bit];
        }
    }

    int findMaxXOR(int num) {
        TrieNode node = root;
        int maxXOR = 0;
        for (int i = 31; i >= 0; i--) {
            int bit = (num >> i) & 1;
            if (node.children[1 - bit] != null) { // Try opposite bit for max XOR
                maxXOR |= (1 << i);
                node = node.children[1 - bit];
            } else {
                node = node.children[bit];
            }
        }
        return maxXOR;
    }
}

public class MaximumXOR {
    public static int findMaximumXOR(int[] nums) {
        Trie trie = new Trie();
        for (int num : nums)
            trie.insert(num);
        
        int maxXOR = 0;
        for (int num : nums)
            maxXOR = Math.max(maxXOR, trie.findMaxXOR(num));
        
        return maxXOR;
    }

    public static void main(String[] args) {
        int[] nums = {3, 10, 5, 25, 2, 8};
        System.out.println("Maximum XOR: " + findMaximumXOR(nums));
    }
}
```

## Complexity Analysis
- **Insertion**: O(N * 32) = O(N), where N is the number of elements.
- **XOR Query**: O(N * 32) = O(N).
- **Overall Complexity**: **O(N)** (efficient compared to brute force O(N^2)).

## Summary
- We use a **Trie** to store binary representations of numbers.
- We efficiently determine the **maximum XOR** for each number in the array.
- The approach runs in **O(N)** time, making it suitable for large inputs.

This method is widely used in competitive programming and technical interviews! 🚀
