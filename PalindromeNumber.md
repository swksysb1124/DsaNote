# 9. Palindrome Number

Given an integer `x`, return *`true` if `x` is a **palindrome**, and `false` otherwise*.


**Example 1:**
```
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
```
**Example 2:**
```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```
**Example 3:**
```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
``` 

**Constraints:**

- -2<sup>31</sup> &le; `x` &le; 2<sup>31</sup> - 1

## Solution

第一種方法：雙指針，把 x 當成字串，從頭尾兩端掃描每一個字元，發現不同表示不是 palindrome。
```kotlin
class Solution {
    fun isPalindrome(x: Int): Boolean {
        if (x < 0) return false

        val str = x.toString()
        val len = str.length

        var left = 0
        var right = len - 1
        while (left < right) {
            if (str[left] != str[right]) return false
            left++
            right--
        }
        return true
    }
}
```

第二種做法：因為是整數，我們可以取得 reversed 數目，最後來比對是否相同。
```kotlin
class Solution {
    fun isPalindrome(x: Int): Boolean {
        if (x < 0) return false
        var reversed = 0
        var original = x
        
        while (original != 0) {
            val digit = original % 10
            reversed = reversed * 10 + digit
            original /= 10
        }

        return reversed == x
    }
}
```
