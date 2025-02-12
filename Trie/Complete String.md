# Understanding the Complete String Problem

## Problem Statement

The task is to identify the longest **"complete string"** from a given array of strings. A string is defined as a complete string if **all of its prefixes** are also present in the array. If multiple strings of the same length exist, the lexicographically smallest one should be returned. If no complete string exists, the output should be **"none"**.

## Definitions

- **Complete String**: A string where every prefix is also present in the array.
- **Prefix**: A substring that starts from the beginning of the string and can be of any length from 1 to the length of the string.

## Example

Given an array of strings, consider the string **"ninja"**. Its prefixes are:

```
n
ni
nin
ninj
ninja
```

All these prefixes must be present in the array for "ninja" to be considered a complete string. If no such string exists, return **"none"**.

---

## Approach to Solve the Problem

To solve this problem, the following steps can be taken:

1. **Utilize a Trie data structure** to store all the strings from the array.
2. **Insert each string** into the Trie, ensuring that each character is linked correctly.
3. **For each string, check** if all its prefixes exist in the Trie.
4. **Keep track of the longest complete string found**, updating it if a longer or lexicographically smaller string is found.

---

## Trie Data Structure

A **Trie** is a tree-like data structure that is used to store a dynamic set of strings. Each node represents a character, and paths down the tree represent prefixes of the strings. The key components of a Trie include:

- An **array of size 26** (for each letter of the alphabet) to hold references to child nodes.
- A **boolean flag** to indicate if a word ends at that node.

### Insertion Process

To insert a word into the Trie:

1. Start at the **root node**.
2. For each character in the word:
   - Check if it exists in the current node's children.
   - If it does not exist, **create a new node** and link it.
   - Move to the child node corresponding to the character.
3. Once the last character is reached, **mark the node's flag as true**.

### Checking for Complete Strings

To determine if a string is a complete string:

1. Start at the **root of the Trie**.
2. For each character in the string:
   - Check if it exists in the Trie.
   - If any character does not exist, the string is **not complete**.
3. If all characters exist and the **final node's flag is true**, the string is **complete**.

---

## Code Implementation (C++)

```cpp
#include <bits/stdc++.h> 

struct TrieNode {
    TrieNode* children[26];
    bool isEndOfWord;
    
    TrieNode() {
        isEndOfWord = false;
        for (int i = 0; i < 26; i++) {
            children[i] = nullptr;
        }
    }
};

class Trie {
public:
    TrieNode* root;
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children[ch - 'a']) {
                node->children[ch - 'a'] = new TrieNode();
            }
            node = node->children[ch - 'a'];
        }
        node->isEndOfWord = true;
    }
    
    bool isCompleteString(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            node = node->children[ch - 'a'];
            if (!node || !node->isEndOfWord) {
                return false;
            }
        }
        return true;
    }
};
string completeString(int n, vector<string> &a){
    // Write your code here.

    Trie trie;
    for (string word : a) {
        trie.insert(word);
    }
    
    string longest = "";
    for (string word : a) {
        if (!trie.isCompleteString(word)) {
            continue;
        }
        if (word.length() > longest.length()) {
            longest = word;
        }else if((word.length() == longest.length() && word < longest)){
                longest = word;

        }
    }

    if (longest.size() == 0) {
      longest = "None";
    }

    return longest;
}
```

---

## Time and Space Complexity

- **Insertion into Trie**: `O(n * m)`, where:
  - `n` is the number of words in the array.
  - `m` is the average length of a word.
- **Checking if a word is complete**: `O(m)`.
- **Overall Complexity**: `O(n * m)`.
- **Space Complexity**:
  - Worst case: `O(26^k)`, where `k` is the length of the longest string.
  - In practice: much less due to shared prefixes in the Trie.



