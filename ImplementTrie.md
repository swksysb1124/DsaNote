# 208. Implement Trie (Prefix Tree)

A **trie** (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

- `Trie()` Initializes the trie object.
- `void insert(String word)` Inserts the string `word` into the trie.
- `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
- `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.
 

**Example 1:**
```
Input
["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
[[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
Output
[null, null, true, false, true, null, true]

Explanation
Trie trie = new Trie();
trie.insert("apple");
trie.search("apple");   // return True
trie.search("app");     // return False
trie.startsWith("app"); // return True
trie.insert("app");
trie.search("app");     // return True
```

**Constraints:**

- `1 <= word.length, prefix.length <= 2000`
- `word` and `prefix` consist only of lowercase English letters.
At most $3 * 10^4$ calls **in total** will be made to `insert`, `search`, and `startsWith`.

## Solution
```kotlin
class Trie() {
    var isWord: Boolean = false
    val children = Array<Trie?>(26) { null }

    fun insert(word: String) {
        var node = this
        for (ch in word) {
            if (node.children[ch - 'a'] == null) {
                node.children[ch - 'a'] = Trie()
            }
            node = node.children[ch - 'a']!!
        }
        node.isWord = true
    }

    fun search(word: String): Boolean {
        var node = this
        for (ch in word) {
            if (node.children[ch - 'a'] == null) return false
            node = node.children[ch - 'a']!!
        }
        return node.isWord
    }

    fun startsWith(prefix: String): Boolean {
        var node = this
        for (ch in prefix) {
            if (node.children[ch - 'a'] == null) return false
            node = node.children[ch - 'a']!!
        }
        return true
    }

}
```
