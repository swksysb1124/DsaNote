# 792. Number of Matching Subsequences

Given a string `s` and an array of strings `words`, return the number of `words[i]` that is a subsequence of `s`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

For example, `"ace"` is a subsequence of `"abcde"`.
 

**Example 1:**
```
Input: s = "abcde", words = ["a","bb","acd","ace"]
Output: 3
Explanation: There are three strings in words that are a subsequence of s: "a", "acd", "ace".
```
**Example 2:**
```
Input: s = "dsahjpjauf", words = ["ahjpjau","ja","ahbwzgqnuk","tnmlanowax"]
Output: 2
``` 

**Constraints:**

- $1 <= s.length <= 5 * 10^4$
- `1 <= words.length <= 5000`
- `1 <= words[i].length <= 50`
- `s` and `words[i]` consist of only lowercase English letters.

## Solution

**Naive approach**

用迴圈掃描 `words` 中每一個字，跟 `s` 比對看是否是 **subsequence** 並記錄數量。

 判定 `w` 是否為 `s` 的 **subsequence** 方式: 使用雙指標分指向 `s` 跟 `w`，只有在 `w[i] == s[j]`，`i` 才位移，`j` 則是每次均會位移一次。

```kotlin
class Solution {
    fun numMatchingSubseq(s: String, words: Array<String>): Int {
        var sum = 0
        for (word in words) {
            if (isSubseq(word, s)) {
                sum++
            }
        }
        return sum
    }
    // 判對 w 是否是 s 的 subsequece
    private fun isSubseq(w: String, s: String): Boolean {
        var i = 0 // 指向 t
        var j = 0 // 指向 j
        val w_ca = s.toCharArray()
        val s_ca = w.toCharArray()
        while (i < w_ca.size && j < s_ca.size) { // 如果 i, j 皆有效
            if (w_ca[i] == s_ca[j]) { // 只有在 w[i] 跟 s[j] 相等時，i 才進到下一位
 &                i++
            }
            j++ // j 每次都進到下一位
        }

        return i == w_ca.size // 最後判對 i 有沒有走到底
    }
}
```

**Emhancement**

```kotlin
class Solution {
    fun numMatchingSubseq(s: String, words: Array<String>): Int {
        val freqOfWords = HashMap<String, Int>()
        for (word in words) {
            val freq = freqOfWords[word] ?: 0
            freqOfWords[word] = freq + 1
        }

        var sum = 0
        for (key in freqOfWords.keys) {
            if (isSubseq(key, s)) {
                sum += (freqOfWords[key] ?: 0)
            }
        }
        return sum
    }

    private fun isSubseq(t: String, s: String): Boolean {
        var i = 0
        var j = 0
        val t_ca = t.toCharArray()
        val s_ca = s.toCharArray() 
        while (i < t_ca.size && j < s_ca.size) {
            if (t_ca[i] == s_ca[j]) {
                i++
            }
            j++
        }
        return i == t_ca.size
    }
}
```

**HashMap + Binary Search**

使用 `HashMap` 紀錄 `s` 各字元 及 出現的 `index`。 

一樣用迴圈比對 `words` 中每一個 `w` 是否為 `s` 的 **subsequence**。

判定方式為：

掃描 `w` 每個 字元，查該字元的 `index`，如果根本不存在字元，就判定 `false`。

如果存在，判定是否有 nextIndex：在這些 `indices` 中找到**最小但比 `preIndex` 大的值** (**upper bond**)，作為 nextIndex。如果找不到 `nextIndex` 也判定 `false`。

如果最後掃描完都沒事，判定成功。

```kotlin
class Solution {
    fun numMatchingSubseq(s: String, words: Array<String>): Int {
        val charArray = s.toCharArray()
        val charToIndices = HashMap<Char, MutableList<Int>>()
        for ((i, c) in charArray.withIndex()) {
            charToIndices.putIfAbsent(c, mutableListOf<Int>())
            charToIndices[c]?.add(i)
        }

        var sum = 0
        for (word in words) {
            if (isSubSeq(word, charToIndices)) {
                sum++
            }
        }
        return sum
    }

    private fun isSubSeq(
        word: String, 
        charToIndices: HashMap<Char, MutableList<Int>>
    ): Boolean {
        var preIndex = -1
        for (c in word.toCharArray()) {
            val indices = charToIndices[c] ?: return false
            val nextIndex = upperBond(preIndex, indices)
                .takeIf { it != -1 } ?: return false
            preIndex = nextIndex
        }
        return true
    }

    // find min index in pos, such that pos[index] > target
    private fun upperBond(target: Int, pos: List<Int>): Int {
        var left = 0
        var right = pos.size - 1
        while (left <= right) {
            val mid = left + (right - left)/2
            if (pos[mid] == target) {
                left = mid + 1
            } else if (pos[mid] > target) {
                right = mid - 1
            } else if (pos[mid] < target) {
                left = mid + 1
            }
        }
        return if (left != pos.size) pos[left] else -1 
    }
}
```
