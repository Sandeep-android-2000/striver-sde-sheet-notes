# **Word Ladder Problem**

## **Problem Statement**
A transformation sequence from a word `beginWord` to a word `endWord` using a dictionary `wordList` is a sequence of words:  

`beginWord -> s1 -> s2 -> ... -> sk`  

such that:  

1. Every adjacent pair of words differs by exactly **one** letter.
2. Every `si` for `1 <= i <= k` is in `wordList` (except `beginWord`, which does not need to be in `wordList`).
3. `sk == endWord`.

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return **the number of words in the shortest transformation sequence** from `beginWord` to `endWord`, or `0` if no such sequence exists.

---

## **Example 1**
**Input:**  
```plaintext
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log","cog"]
```
**Output:**  
```plaintext
5
```
**Explanation:**  
One of the shortest transformation sequences is:
```plaintext
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
```
The sequence length is **5**.

---

## **Example 2**
**Input:**  
```plaintext
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]
```
**Output:**  
```plaintext
0
```
**Explanation:**  
The `endWord` "cog" is not in `wordList`, so no valid transformation sequence exists.

---

## **Approach**
### **1. Breadth-First Search (BFS)**
- Since we are looking for the **shortest** transformation sequence, **BFS** is the optimal approach.
- BFS ensures that the first time we reach `endWord`, it is through the shortest path.

### **2. Steps Involved**
1. Use a **queue** to store words along with their transformation step count.
2. Use an **unordered set** to store `wordList` for quick lookups.
3. Start BFS from `beginWord`:
   - Generate all possible words by changing each letter (`a-z`).
   - If the new word is in `wordList`, add it to the queue and remove it from the set.
   - Continue BFS until `endWord` is reached or no more transformations are possible.

---

## **Implementation**
```cpp
class Solution {
public:
    int ladderLength(string startWord, string targetWord, vector<string>& wordList) {
        queue<pair<string, int>> q;
        q.push({startWord, 1});

        unordered_set<string> st(begin(wordList), end(wordList));
        st.erase(startWord);

        while (!q.empty()) {
            auto it = q.front();
            q.pop();

            string word = it.first;
            int steps = it.second;

            if (word == targetWord)
                return steps;

            for (int i = 0; i < word.size(); i++) {
                char orgChar = word[i];

                for (char c = 'a'; c <= 'z'; c++) {
                    word[i] = c;
                    if (st.find(word) != st.end()) {
                        st.erase(word);
                        q.push({word, steps + 1});
                    }
                }

                word[i] = orgChar;
            }
        }
        return 0;
    }
};
```

---

## **Time Complexity Analysis**
### **Let:**
- `L` = Length of each word
- `N` = Number of words in `wordList`

### **Complexity Breakdown:**
1. **Building the set:** `O(N)`
2. **BFS traversal:** Each word generates up to `L * 25` new words.
   - In the worst case, we visit all `N` words.
   - **Total complexity:** \( O(N \times L \times 26) \)  
   - Since `26` is a constant, it simplifies to:  
     **`O(N * L)`**

### **Space Complexity**
- **Queue:** Stores at most `N` words → `O(N)`
- **Set:** Stores `N` words → `O(N)`
- **Total space complexity:** **`O(N)`**

---

## **Optimizations**
✅ **Use a set (`unordered_set`) for fast lookups (`O(1)`).**  
✅ **Erase words from the set when visited to avoid revisits.**  
✅ **Use BFS for shortest path discovery.**  

---

## **Summary**
| Approach | Time Complexity | Space Complexity |
|----------|----------------|------------------|
| BFS (Optimal) | `O(N * L)` | `O(N)` |

This solution efficiently finds the shortest transformation sequence while ensuring **minimal processing overhead**. 🚀
