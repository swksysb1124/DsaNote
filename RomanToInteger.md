# 13. Roman to Integer

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

|Symbol  |Value |
|:---|:---|
|I | 1|
|V   |     5|
|X   |     10|
|L   |     50|
|C   |     100|
|D   |     500|
|M   |     1000|

For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as` XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 

- `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 

- `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.
  
Given a roman numeral, convert it to an integer.


**Example 1:**
```
Input: s = "III"
Output: 3
Explanation: III = 3.
```

**Example 2:**
```
Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

**Example 3:**
```
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

**Constraints:**

- `1 <= s.length <= 15`
- `s` contains only the characters `('I', 'V', 'X', 'L', 'C', 'D', 'M')`.
- It is **guaranteed** that s is a valid roman numeral in the range `[1, 3999]`.

## Solution

核心概念：很簡單，就是依照題意轉換羅馬文字為數字，而當羅馬文字為 `I`、`X`、`C`，同時確認下一個羅馬文字來看是否要特別處理。

```kotlin
class Solution {
    fun romanToInt(s: String): Int {
        if (s.isEmpty()) return 0
        var i = 0
        val n = s.length
        var sum = 0

        while (i < n) {
            if (s[i] == 'I') {
                if (i + 1 < n && s[i + 1] == 'V') {
                    sum += 4
                    i++
                } else if (i + 1 < n && s[i + 1] == 'X'){
                    sum += 9
                    i++
                } else {
                    sum += 1
                }
            } else if (s[i] == 'V') {
                sum += 5
            } else if (s[i] == 'X') {
                if (i + 1 < n && s[i + 1] == 'L') {
                    sum += 40
                    i++
                } else if (i + 1 < n && s[i + 1] == 'C'){
                    sum += 90
                    i++
                } else {
                    sum += 10
                }
            } else if (s[i] == 'L') {
                sum += 50
            } else if (s[i] == 'C') {
                if (i + 1 < n && s[i + 1] == 'D') {
                    sum += 400
                    i++
                } else if (i + 1 < n && s[i + 1] == 'M'){
                    sum += 900
                    i++
                } else {
                    sum += 100
                }
            } else if (s[i] == 'D') {
                sum += 500
            } else if (s[i] == 'M') {
                sum += 1000
            }
            i++
        }
        return sum
    }
}
```

**進階**

假設我們先不特別處理`I`、`X`、`C`，而是把他們當 1, 10, 100 處理，`IV` 我們會處理成 6 而不是 4，`IX` 會處理成 11 而不是 9。也就是會多加 2。同理，`XL`跟`XC` 會多算 20，而 `CD`跟`CM` 會多算 200。所以我們先不特別處理 `I`、`X`、`C`，將羅馬文字轉換成數字加總起來，最後在針對 `I`、`X`、`C` 減去多加的部分。

```kotlin
class Solution {
    fun romanToInt(s: String): Int {
        if (s.isEmpty()) return 0
        val n = s.length
        var sum = 0

        for (i in 0..<n) {
            val roman = s[i]
            when (roman) {
                'I' -> sum += 1
                'V' -> sum += 5
                'X' -> sum += 10
                'L' -> sum += 50
                'C' -> sum += 100
                'D' -> sum += 500
                'M' -> sum += 1000
            }
        } 

        for (i in 0..<n-1) {
            val roman = s[i]
            val next = s[i + 1]
            when {
                roman == 'I' && (next == 'V' || next == 'X') -> sum -= 2  
                roman == 'X' && (next == 'L' || next == 'C')-> sum -= 20
                roman == 'C' && (next == 'D' || next == 'M')-> sum -= 200
            }
        }

        return sum
    }
}
```