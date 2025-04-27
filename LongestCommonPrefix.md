# 14. Longest Common Prefix

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

 

**Example 1:**
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```
**Example 2:**
```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
``` 

**Constraints:**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` consists of only lowercase English letters if it is non-empty.

## Solution
```kotlin
class Solution {
    fun longestCommonPrefix(strs: Array<String>): String {
        // corner cases: 考慮都沒有字串 或是 單個字串
        if (strs.isEmpty()) return ""
        if (strs.size == 1) return strs[0]
    
        // 比對第一字串跟第二字串來初始化 lcp
        var lcp = generateLcp(strs[0], strs[1])

        for (i in 2 until strs.size) {
            // 比對 lcp 跟接下來的字串來更新 lcp
            lcp = generateLcp(lcp, strs[i])
        }
        return lcp
    }

    private fun generateLcp(s1: String, s2: String): String {
        var i = 0
        while (i < s1.length && i < s2.length) {
            if (s1[i] == s2[i]) {
                i++
            } else { 
                break
            }
        }
        return s1.substring(0, i)
    }
}
```