# 224. Basic Calculator

Given a string `s` representing a valid expression, implement a basic calculator to evaluate it, and return *the result of the evaluation*.

**Note**: You are **not** allowed to use any built-in function which evaluates strings as mathematical expressions, such as `eval()`.

 

**Example 1:**
```
Input: s = "1 + 1"
Output: 2
```
**Example 2:**
```
Input: s = " 2-1 + 2 "
Output: 3
```
**Example 3:**
```
Input: s = "(1+(4+5+2)-3)+(6+8)"
Output: 23
``` 

**Constraints:**

- $1 <= s.length <= 3 * 10^5$
- `s` consists of digits, `'+'`, `'-'`, `'('`, `')'`, and `' '`.
- `s` represents a valid expression.
- `'+'` is **not** used as a unary operation (i.e., `"+1"` and `"+(2 + 3)"` is invalid).
- `'-'` could be used as a unary operation (i.e., `"-1"` and `"-(2 + 3)"` is valid).
- There will be no two consecutive operators in the input.
- Every number and running calculation will fit in a signed 32-bit integer.

## Solution

核心觀念：

逐一掃描每個字元，
- 如果是**數字**，就持續解析出數字多少，最後加總到結果中。
- 如果是 `'+'`，紀錄目前 **sign** 為 `1`
- 如果是 `'-'`，紀錄目前 **sign** 為 `-1`
- 如果是 `'(`'，將目前 res 跟 sign 放到 stack
- 如果是 `')'`，更新 `res = stack.pop() * res + stack.pop()`。

```kotlin
class Solution {
    fun calculate(s: String): Int {
        var res = 0
        val stack = LinkedList<Int>()
        var i = 0
        var sign = 1

        while (i < s.length) {
            val c = s[i]
            if (c.isDigit()) {
                var num = 0
                while (i < s.length && s[i].isDigit()) {
                    num = 10 * num + (s[i] - '0')
                    i++
                }
                res += sign * num
                // 回到解析出的數字的個位數
                i--
            } else if (c == '+') {
                sign = 1
            } else if (c == '-') {
                sign = -1
            } else if (c == '(') {
                stack.push(res)
                stack.push(sign)
                res = 0
                sign = 1
            } else if (c == ')') {
                res = stack.pop() * res + stack.pop()
            }
            i++
        }

        return res
    }
}
```