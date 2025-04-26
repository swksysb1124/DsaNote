# 299. Bulls and Cows

You are playing the **Bulls and Cows** game with your friend.

You write down a secret number and ask your friend to guess what the number is. When your friend makes a guess, you provide a hint with the following info:

- The number of "bulls", which are digits in the guess that are in the correct position.

- The number of "cows", which are digits in the guess that are in your secret number but are located in the wrong position. Specifically, the non-bull digits in the guess that could be rearranged such that they become bulls.

Given the secret number `secret` and your friend's guess `guess`, return *the hint for your friend's guess*.

The hint should be formatted as `"xAyB"`, where `x` is the number of bulls and `y` is the number of cows. Note that both `secret` and `guess` may contain duplicate digits.

 

**Example 1:**
```
Input: secret = "1807", guess = "7810"
Output: "1A3B"
Explanation: Bulls are connected with a '|' and cows are underlined:
"1807"
  |
"7810"
```
**Example 2:**
```
Input: secret = "1123", guess = "0111"
Output: "1A1B"
Explanation: Bulls are connected with a '|' and cows are underlined:
"1123"        "1123"
  |      or     |
"0111"        "0111"
Note that only one of the two unmatched 1s is counted as a cow since the non-bull digits can only be rearranged to allow one 1 to be a bull.
``` 

**Constraints:**

- `1 <= secret.length, guess.length <= 1000`
- `secret.length == guess.length`
- `secret` and `guess` consist of digits only.


## Solution

核心觀念：

分兩次掃描

- 第一次只針對**同位置、同數字**掃描，並計算 x 值，同時紀錄排除同位置、同數字之外的 secret 跟 guess。
  
- 針對排除同位置、同數字的 secret 跟 guess 再一次掃描，這是只計算在相同數字的數量。

```kotlin
class Solution {
    fun getHint(secret: String, guess: String): String {
        var x = 0
        var y = 0
        val secretList = mutableListOf<Char>()
        val guessList = mutableListOf<Char>()

        val map = HashMap<Char, Int>()

        // 先處理同位置，同數字
        // 將他們排除
        for (i in 0 until secret.length) {
            if (secret[i] == guess[i]) {
                x++
            } else {
                secretList.add(secret[i])
                map[secret[i]] = (map[secret[i]] ?: 0) + 1 

                guessList.add(guess[i])
            }
        }
        
        // 第二是只有確定，是否有相同數字
        for (c in guessList) {
            if ((map[c] ?: 0) > 0) {
                y++
                map[c] = (map[c] ?: 0) - 1
            }
        }
        
        return "${x}A${y}B"
    }
}
```

近一步延伸，第二遍掃描只在意相同數字的數目，可以使用 IntArray(10) 來記錄 secret & guess 各數字的字數，比對各數字最小，再加總起來。

```kotlin
class Solution {
    fun getHint(secret: String, guess: String): String {
        var x = 0
        var y = 0
        val secretDigits = IntArray(10) { 0 }
        val guessDigits = IntArray(10) { 0 }

        // 先處理同位置，同數字
        for (i in 0 until secret.length) {
            if (secret[i] == guess[i]) {
                x++
            } else {
                // 計算 secret & guess 各數字的個數
                secretDigits[secret[i] - '0'] = secretDigits[secret[i] - '0'] + 1
                guessDigits[guess[i] - '0'] = guessDigits[guess[i] - '0'] + 1 
            }
        }
        
        for (i in 0..9) {
            y += min(secretDigits[i], guessDigits[i])
        }
        
        return "${x}A${y}B"
    }
}
```