# 5. Longest Palindromic Substring

Given a string `s`, return the longest **palindromic substring** in `s`.

**Example 1:**
```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

**Example 2:**
```
Input: s = "cbbd"
Output: "bb"
```

**Constraints:**
- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.

## Solution

### Simple approach
```kotlin
class Solution {
    private var maxLen = 0
    private var maxPalin = ""

    fun longestPalindrome(s: String): String {
        val n = s.length
        val chars = s.toCharArray()
        for (i in 0 until n) {
            findLongestPalindrome(chars, n, start = i, offset = 0)
            findLongestPalindrome(chars, n, start = i, offset = 1)
        }
        return maxPalin     
    }

    private fun findLongestPalindrome(chars: CharArray, n: Int, start: Int, offset: Int) {
        var left = start
        var right = start + offset

        while(
            // left & right 必須有效
            left >= 0 && right < n 
            // 左右兩端要一樣
            && chars[left] == chars[right] 
        ) {
            left--
            right++
        }

        // right & left 現在比符合條件多一次位移
        val len = right - left - 1
    
        if (len > maxLen) {
            maxLen = len
            maxPalin = String(chars, left + 1, len)
        }
    }
}
```

|Time Complexity|Space Complexity|
|:---:|:---:|
| O(n<sup>2</sup>)|O(n<sup>2</sup>) |


### DP

```
    i   i-1       j-1   j
 s[ a ][ b ][ c ][ b ][ a ]
```

When `j - i >= 2`, if `s[i-1..j-1]` is **palindrome** and `s[i] == s[j]`, then `s[i..j]` is **palindrome**. For eample, `"aba"`, `"abba"`, `"abcba"`.

When `j - i < 2`, if `s[i] == s[j]`, then `s[i..j]` is **palindrome**. For eample, `"a"`, `"aa"`.


```
  s = "ababa"
         j                     j
  [1][?][?][?][?]       [1][0][1][0][1]
  [0][1][?][?][?]       [0][1][0][1][0]
i [0][0][1][?][?]  => i [0][0][1][0][1]
  [0][0][0][1][?]       [0][0][0][1][0]
  [0][0][0][0][1]       [0][0][0][0][1]

Because j >= i, we only consider the up-right part of the dp matrix.
scan i from n - 2 to 0
scan j from i to n - 1  
```
**由下往上，由左而右掃描**
```kotlin
class Solution {
    fun longestPalindrome(s: String): String {
        val n = s.length 
        if (n == 1) return s

        var maxLen = 0
        var result = ""

        // dp[i][j]: if only s[i..j] is polindrome , then s[i..j] is true, or it is false  
        val dp = Array(n) { BooleanArray(n) { false } }
        val chars = s.toCharArray()

        for (i in n - 2 downTo 0) {
            for (j in i until n) {
                if (chars[i] == chars[j] && (j - i < 2 || dp[i + 1][j - 1])) {
                    dp[i][j] = true

                    val len = j - i + 1
                    if (len > maxLen) {
                        maxLen = len
                        result = String(chars, i, len)
                    }
                }
            }
        }

        return result
    }
}
```
|Time Complexity|Space Complexity|
|:---:|:---:|
| O(n<sup>2</sup>)|O(1) |