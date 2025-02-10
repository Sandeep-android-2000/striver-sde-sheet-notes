# Implementing Trie Data Structure: Part Two

## Overview

This section focuses on solving the second question in the Trie playlist, which builds upon the previous question. The main functionalities to be implemented include:

- **Insert**: Add a word to the Trie.
- **Count Words Equal To**: Determine how many words in the Trie match a given word.
- **Count Words Starting With**: Count how many words in the Trie start with a specified prefix.
- **Erase**: Remove a word from the Trie.

## Understanding the Problem

The problem requires the implementation of a Trie data structure with the following functionalities:

- Insert a word into the Trie.
- Count how many words exist that are equal to a given word.
- Count how many words start with a given prefix.
- Erase a specified word from the Trie.

It is important to note that this question is an extension of the previous one, where the focus was on checking for the existence of words with a given prefix.

## Example Walkthrough

To illustrate the functionalities, consider the following example:

1. Insert **"samsung"** twice and **"vivo"**.
2. Count words equal to **"samsung"** → should return **2**.
3. Count words starting with **"vi"** → should return **0** after erasing **"vivo"**.

## Implementation Details

The Trie will consist of nodes that contain:

- **Links** to 26 characters (for each letter of the alphabet).
- An integer variable **endWith** to count how many words end at this node.
- An integer variable **countPrefix** to count how many words start with the prefix represented by this node.

## Insert Functionality

When inserting a word, the algorithm traverses the Trie, creating new nodes as necessary. For each character in the word:

- If the character node does not exist, create it.
- Increment the **countPrefix** for each character node traversed.
- At the end of the word, increment the **endWith** count.

This ensures that the Trie accurately reflects the number of words that end at each node and how many words start with each prefix.

## Count Words Equal To

To count how many words are equal to a given word:

- Start at the root and traverse the Trie according to the characters of the word.
- If a character does not exist, return **0**.
- At the end of the traversal, return the **endWith** count of the last node.

## Count Words Starting With

To count how many words start with a given prefix:

- Similar to counting words equal to a word, traverse the Trie using the prefix.
- If a character does not exist, return **0**.
- At the end of the traversal, return the **countPrefix** of the last node.

## Erase Functionality

To erase a word from the Trie:

- Traverse the Trie as you would for insertion.
- For each character, decrement the **countPrefix** at each node.
- At the end of the word, decrement the **endWith** count.

Note that nodes are not deleted; instead, the counts are adjusted to reflect the removal of the word.

---

## Java Code Implementation

```java
class TrieNode {
    TrieNode[] children;
    int endWith;
    int countPrefix;

    public TrieNode() {
        children = new TrieNode[26]; // 26 letters for lowercase English
        endWith = 0;
        countPrefix = 0;
    }
}

class Trie {
    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

    // Insert a word into the Trie
    public void insert(String word) {
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (node.children[index] == null) {
                node.children[index] = new TrieNode();
            }
            node = node.children[index];
            node.countPrefix++;
        }
        node.endWith++;
    }

    // Count words equal to the given word
    public int countWordsEqualTo(String word) {
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            if (node.children[index] == null) {
                return 0;
            }
            node = node.children[index];
        }
        return node.endWith;
    }

    // Count words starting with the given prefix
    public int countWordsStartingWith(String prefix) {
        TrieNode node = root;
        for (char ch : prefix.toCharArray()) {
            int index = ch - 'a';
            if (node.children[index] == null) {
                return 0;
            }
            node = node.children[index];
        }
        return node.countPrefix;
    }

    // Erase a word from the Trie
    public void erase(String word) {
        if (countWordsEqualTo(word) == 0) return; // Word does not exist
        
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            int index = ch - 'a';
            node = node.children[index];
            node.countPrefix--;
        }
        node.endWith--;
    }

    public static void main(String[] args) {
        Trie trie = new Trie();
        
        trie.insert("samsung");
        trie.insert("samsung");
        trie.insert("vivo");
        
        System.out.println("Count of 'samsung': " + trie.countWordsEqualTo("samsung")); // Output: 2
        System.out.println("Count words starting with 'vi': " + trie.countWordsStartingWith("vi")); // Output: 1
        
        trie.erase("vivo");
        System.out.println("Count words starting with 'vi' after erase: " + trie.countWordsStartingWith("vi")); // Output: 0
    }
}
```

## Complexity Analysis

| Operation                 | Time Complexity |
|--------------------------|----------------|
| Insert                   | O(L)           |
| Count Words Equal To      | O(L)           |
| Count Words Starting With | O(L)           |
| Erase                    | O(L)           |

Where **L** is the length of the word being processed.

## Conclusion

This implementation of the **Trie Data Structure** supports word insertion, counting words by exact match, counting words by prefix, and word deletion efficiently. The approach ensures optimal **O(L)** time complexity for all operations, making it a robust solution for handling dictionary-based queries in text processing and search applications.
