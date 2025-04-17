# 648. Replace Words
## Solution
### Blute force
```kotlin
class Solution {
    fun replaceWords(dictionary: List<String>, sentence: String): String {
        val words = sentence.split(" ").toMutableList()
        for (i in 0 until words.size) {
            val word = words[i]
            var minLen = 101
            var replace = ""
            for (root in dictionary) {
                if (word.length < root.length) continue

                if (word.substring(0, root.length) == root) {
                    if (minLen > root.length) {
                        minLen = root.length
                        replace = root
                    }
                }
            }
            if (replace.isNotEmpty()) {
                words[i] = replace
            }
        }
        return words.joinToString(" ")
    }
}
```

Time complexity: `O(N*M)`

### Trie
```kotlin
class Solution {
    fun replaceWords(dictionary: List<String>, sentence: String): String {
        val root = TrieNode()
        for (prefix in dictionary) {
            insert(root, prefix)
        }
        val words = sentence.split(" ")
        val result = words.toMutableList()
        for (i in 0 until words.size) {
            val word = words[i]
            result[i] = search(root, word)
        }
        return result.joinToString(" ")
    }

    fun insert(root: TrieNode, word: String) {
        var node: TrieNode? = root
        for (c in word.toCharArray()) {
            val i = c - 'a'
            if (node?.children[i] == null) {
                node?.children[i] = TrieNode()
            }
            node = node?.children[i]
        }
        node?.word = word
    }

    fun search(root: TrieNode, word: String): String {
        var node: TrieNode? = root
        for (c in word.toCharArray()) {
            if (node?.word != null) {
                return node.word!!
            }
            val i = c - 'a'
            if (node?.children[i] == null) {
                return word
            }
            node = node.children[i]
        }
        return word
    }
}

class TrieNode {
    val children: Array<TrieNode?> = Array<TrieNode?>(26) { null }
    var word: String? = null
}
```