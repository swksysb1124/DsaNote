# 648. Replace Words

In English, we have a concept called **root**, which can be followed by some other word to form another longer word - let's call this word **derivative**. For example, when the **root** `"help"` is followed by the word `"ful"`, we can form a derivative `"helpful"`.

Given a `dictionary` consisting of many **roots** and a `sentence` consisting of words separated by spaces, replace all the derivatives in the sentence with the **root** forming it. If a derivative can be replaced by more than one **root**, replace it with the **root** that has **the shortest length**.

Return *the `sentence` after the replacement*.

 
**Example 1:**
```
Input: dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"
```
**Example 2:**
```
Input: dictionary = ["a","b","c"], sentence = "aadsfasf absbs bbab cadsfafs"
Output: "a a b c"
```

**Constraints:**

- `1 <= dictionary.length <= 1000`
- `1 <= dictionary[i].length <= 100`
- `dictionary[i]` consists of only lower-case letters.
- `1 <= sentence.length <= 10^6`
- `sentence` consists of only lower-case letters and spaces.
- The number of words in `sentence` is in the range `[1, 1000]`
- The length of each word in `sentence` is in the range `[1, 1000]`
- Every two consecutive words in `sentence` will be separated by exactly one space.
- `sentence` does not have leading or trailing spaces.

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