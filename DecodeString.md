# 394. Decode String

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

The test cases are generated so that the length of the output will never exceed $10^5$.

**Example 1:**
```
Input: s = "3[a]2[bc]"
Output: "aaabcbc"
```
**Example 2:**
```
Input: s = "3[a2[c]]"
Output: "accaccacc"
```
**Example 3:**
```
Input: s = "2[abc]3[cd]ef"
Output: "abcabccdcdcdef"
``` 

**Constraints:**

- `1 <= s.length <= 30`
- `s` consists of lowercase English letters, digits, and square brackets `'[]'`.
- `s` is guaranteed to be **a valid** input.
- All the integers in `s` are in the range `[1, 300]`.

## Solution
1. 使用 `stack` 來處理，逐步掃描 `s` 中每一個字元，並放入 `stack`。
2. 當掃描碰到 `']'`，停止掃描也不將 `']'` 放入，而是將 `stack` 中的字元取出，直到碰到 `'['`，並將取出字元依**倒序方式**串連，作為等一下的**複製字串**。
3. 處理完**複製字串**，處理**複製的數量**。同樣持續從 `stack` 取出字元，只要 `stack` 不為空且為 `'0'` ~ `'9'`字元，表示為數字，同樣以**倒序方式**串連為數字。最後將其轉換為整數型態。
4. 依照剛剛的數字作為倍數，將複製字串複製放到 `stack`。然後繼續掃描。
5. 當全部掃描完成，將 `stack` 全部取出，並依倒序方式組成最後的結果。


```kotlin
class Solution {
    fun decodeString(s: String): String {
        val stack = LinkedList<String>()

        for (c in s.toCharArray()) {
            if (c != ']') {
                // c 轉換成 string，放入 stack
                stack.push(c.toString())
            } else {
                // 生成複製字串
                val sb = StringBuilder()
                while (stack.peek() != "[") {
                    // 依倒序方式放入sb
                    sb.insert(0, stack.pop())
                }

                stack.pop() // pop 掉 "["

                // 生成倍數
                val numSb =  StringBuilder()
                // stack 不為空，且 peek 是介於 "0"~"9"
                while (
                    stack.isNotEmpty() && 
                    stack.peek() >= "0" && 
                    stack.peek() <= "9"
                ) {
                    // 依倒序方式放入sb
                    numSb.insert(0, stack.pop())
                }

                var k = numSb.toString().toInt()
                val encoded = sb.toString()
                while (k > 0) {
                    stack.push(encoded)
                    k--
                }
            }
        }

        val result = StringBuilder()
        while (!stack.isEmpty()) {
            result.insert(0, stack.pop())
        }
        return result.toString()
    }
}
```