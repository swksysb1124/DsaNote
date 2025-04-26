# 3. Longest Substring Without Repeating Characters

Given a string `s`, find the length of the **longest substring** without duplicate characters.

**Example 1:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```
**Example 2:**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```
**Example 3:**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
``` 

**Constraints:**

- $0 <= s.length <= 5 * 10^4$
- `s` consists of English letters, digits, symbols and spaces.

## Solution
```kotlin
class Solution {
    fun lengthOfLongestSubstring(s: String): Int {
        val window = HashMap<Char, Int>()
        val chars = s.toCharArray()
        val n = chars.size
        var left = 0
        var right = 0
        var max = 0

        while (right < n) {
            // 更新 right 
            val c = chars[right]
            right++

            // 更新 window 內容
            window[c] = (window[c] ?: 0) + 1

            // 更新 left
            while ((window[c] ?: 0) > 1) {
                val d = chars[left] 
                left++

                // 更新 window 內容
                window[d] = (window[d] ?: 0) - 1
            }

            // 更新答案
            max = max(max, right - left)
        }

        return max
    }
}
```