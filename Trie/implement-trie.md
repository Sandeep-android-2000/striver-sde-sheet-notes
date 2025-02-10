# Trie Data Structure - Core Concepts and Implementation

## Intuition
### 1. Trie (Prefix Tree):
- Efficient for operations involving strings, like search, insert, delete, and prefix matching.
- Similar to graphs; each node represents a character, with edges connecting to subsequent characters.

### 2. Key Operations in Trie:
- **Insert**: Add words character by character.
- **Search**: Check if a word exists.
- **StartsWith**: Check if any word starts with a given prefix.
- **Delete** (optional): Remove a word.

## Logic
### 1. Node Structure:
- `isEnd`: Boolean flag to indicate end of a word.
- `children`: Array or hashmap to store child nodes for each character ('a' to 'z').

### 2. Trie Structure:
- Root node initiates the Trie.
- Traversal for each operation starts from the root.

## Operations Breakdown
### 1. Insert Operation:
- Traverse character by character.
- Create nodes if they don’t exist.
- Mark `isEnd = true` for the last character.

### 2. Search Operation:
- Similar to insert but checks for existence.
- Return `true` if end node’s `isEnd` is true, else `false`.

### 3. StartsWith Operation:
- Traverse using prefix.
- If traversal is successful, return `true`.

## Code Implementation (C++)
```cpp
struct TrieNode {
    bool isEnd;
    TrieNode* children[26];
    
    TrieNode() {
        isEnd = false;
        for (int i = 0; i < 26; i++)
            children[i] = nullptr;
    }
};

class Trie {
private:
    TrieNode* root;

public:
    Trie() {
        root = new TrieNode();
    }

    // Insert a word into the trie.
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int index = c - 'a';
            if (node->children[index] == nullptr) {
                node->children[index] = new TrieNode();
            }
            node = node->children[index];
        }
        node->isEnd = true;
    }

    // Search a word in the trie.
    bool search(string word) {
        TrieNode* node = root;
        for (char c : word) {
            int index = c - 'a';
            if (node->children[index] == nullptr) {
                return false;
            }
            node = node->children[index];
        }
        return node->isEnd;
    }

    // Check if any word starts with the given prefix.
    bool startsWith(string prefix) {
        TrieNode* node = root;
        for (char c : prefix) {
            int index = c - 'a';
            if (node->children[index] == nullptr) {
                return false;
            }
            node = node->children[index];
        }
        return true;
    }
};
```

## Key Points
- **TrieNode Structure:**
  - `isEnd` to mark end of a word.
  - `children` to store references for each character.
  
- **Insert Operation:**
  - Traverse through each character.
  - Create new nodes if necessary.
  - Mark the last node as end of word.
  
- **Search Operation:**
  - Traverse similar to insert.
  - Return `true` only if last node's `isEnd` is `true`.
  
- **StartsWith Operation:**
  - Traverse only till the prefix length.
  - Return `true` if traversal succeeds.

## Complexity Analysis
- **Time Complexity:**  
  - Insert, Search, StartsWith: `O(L)`, where `L` is the length of the word/prefix.
- **Space Complexity:**  
  - `O(26^L)` in worst case, where `L` is the length of the longest word.

## Conclusion
- Tries are powerful for solving problems involving prefixes and fast lookups.
- Understanding the core functions allows solving complex problems efficiently.
