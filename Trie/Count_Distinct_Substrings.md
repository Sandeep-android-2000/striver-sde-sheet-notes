# Number of Distinct Substrings in a String | Trie | C++ | Java

## Problem Statement
Given a string `s`, the task is to find the number of distinct substrings of `s`, including the empty string.

## Explanation
A substring is any contiguous sequence of characters within a string. To efficiently count the number of distinct substrings, we can use the **Trie** data structure. 

### Approach
1. **Insert all suffixes into a Trie:**
   - Consider all suffixes of the string and insert them into a Trie.
   - Each unique path in the Trie corresponds to a distinct substring.
   
2. **Count unique nodes in the Trie:**
   - The number of nodes in the Trie (excluding the root) gives the count of distinct substrings.
   
### Trie Data Structure
A Trie consists of nodes where:
- Each node represents a character.
- Each node has links to 26 children (for lowercase English letters).
- A node can mark the end of a word.

### Time Complexity
- **Insertion:** Each insertion of a suffix takes `O(m)`, where `m` is the length of the suffix.
- **Total Complexity:** `O(n^2)`, where `n` is the length of the string (since we insert `n` suffixes).

---
## C++ Implementation
```cpp
#include <iostream>
using namespace std;

struct TrieNode {
    TrieNode* children[26];
    TrieNode() {
        for (int i = 0; i < 26; i++) children[i] = nullptr;
    }
};

class Trie {
public:
    TrieNode* root;
    Trie() { root = new TrieNode(); }
    
    int insert(string s) {
        TrieNode* node = root;
        int newNodes = 0;
        for (char c : s) {
            int index = c - 'a';
            if (!node->children[index]) {
                node->children[index] = new TrieNode();
                newNodes++;
            }
            node = node->children[index];
        }
        return newNodes;
    }
};

int countDistinctSubstrings(string s) {
    Trie trie;
    int count = 0;
    for (int i = 0; i < s.length(); i++) {
        count += trie.insert(s.substr(i));
    }
    return count + 1; // Including empty string
}

int main() {
    string s = "ababa";
    cout << "Number of distinct substrings: " << countDistinctSubstrings(s) << endl;
    return 0;
}
```

---
## Java Implementation
```java
class TrieNode {
    TrieNode[] children;
    public TrieNode() {
        children = new TrieNode[26];
    }
}

class Trie {
    TrieNode root;
    public Trie() {
        root = new TrieNode();
    }
    
    public int insert(String s) {
        TrieNode node = root;
        int newNodes = 0;
        for (char c : s.toCharArray()) {
            int index = c - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
                newNodes++;
            }
            node = node.children[index];
        }
        return newNodes;
    }
}

public class DistinctSubstrings {
    public static int countDistinctSubstrings(String s) {
        Trie trie = new Trie();
        int count = 0;
        for (int i = 0; i < s.length(); i++) {
            count += trie.insert(s.substring(i));
        }
        return count + 1; // Including empty string
    }

    public static void main(String[] args) {
        String s = "ababa";
        System.out.println("Number of distinct substrings: " + countDistinctSubstrings(s));
    }
}
```

---
## Summary
- **Trie is an efficient way** to count distinct substrings.
- **Time complexity**: `O(n^2)`, due to inserting all suffixes.
- **Space complexity**: `O(n^2)`, since the worst case is storing all substrings.

This approach ensures that we correctly count all unique substrings of a given string. 🚀
