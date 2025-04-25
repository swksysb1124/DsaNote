# 552. Student Attendance Record II

An attendance record for a student can be represented as a string where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

- `'A'`: Absent.
  
- `'L'`: Late.
  
- `'P'`: Present.

Any student is eligible for an attendance award if they meet **both** of the following criteria:

- The student was absent (`'A'`) for **strictly** fewer than 2 days **total**.
  
- The student was **never** late (`'L'`) for 3 or more **consecutive** days.

Given an integer `n`, return *the **number** of possible attendance records of length `n` that make a student eligible for an attendance award. The answer may be very large, so return it **modulo** $10^9 + 7$*.

 

**Example 1:**
```
Input: n = 2
Output: 8
Explanation: There are 8 records with length 2 that are eligible for an award:
"PP", "AP", "PA", "LP", "PL", "AL", "LA", "LL"
Only "AA" is not eligible because there are 2 absences (there need to be fewer than 2).
```

**Example 2:**
```
Input: n = 1
Output: 3
```

**Example 3:**
```
Input: n = 10101
Output: 183236316
``` 

**Constraints:**

- $1 <= n <= 10^5$


## Solution

核心概念：

我們以 `f(i - 1, a, l)` 表示可以得獎的數量，其中 `i`為出席紀錄，`a` 表示缺席天數，`l` 表示連續遲到天數。

第 `i` 天可以選擇
- 選擇 `P`(出席)，`f(i, a, 0) = f(i, a, 0) + f(i - 1, a, l)`。`l` 之所以為零，是因為連續天數中斷，回到 `0`。
  
- 如果 `a < 1`，可選擇 `A`(缺席)，`f(i, a + 1, 0) = f(i, a + 1, 0) + f(i - 1, a, l)`。`l` 之所以為零，是因為連續天數中斷，回到 `0`。
  
- 如果 `l < 2`，可選擇 `L`(遲到)，`f(i, a, l + 1) = f(i, a,  l + 1) + f(i - 1, a, l)`。




**DFS**

```kotlin
class Solution {
    fun checkRecord(n: Int): Int {
        val mod = 1000_000_000 + 7

        fun numberOfAward(i: Int, absent: Int, late: Int): Long {
            if (absent >= 2 || late >= 3) return 0
            if (i == 0) return 1

            var res = numberOfAward(i - 1, absent, 0) // 有出席
            res += numberOfAward(i - 1, absent + 1, 0) // 缺席
            res += numberOfAward(i - 1, absent, late + 1) // 遲到
            
            return res
        }

        return numberOfAward(n, 0, 0).toInt()
    }
}
```

**DP**

```kotlin
class Solution {
    fun checkRecord(n: Int): Int {
        val mod = 1_000_000_007

        val dp = Array(n + 1) { Array(2) { LongArray(3) } }

        // 初始狀態：第0天，0缺席，0遲到，有1種方式（空字串）
        dp[0][0][0] = 1L

        for (i in 1..n) {
            for (a in 0..1) {
                for (l in 0..2) {
                    val count = dp[i - 1][a][l]

                    // 加上 P（出席），會讓連續 L 歸 0
                    dp[i][a][0] = (dp[i][a][0] + count) % mod

                    // 加上 A（缺席），只能在 a == 0 時加入
                    if (a < 1) {
                        dp[i][a + 1][0] = (dp[i][a + 1][0] + count) % mod
                    }

                    // 加上 L（遲到），最多2次
                    if (l < 2) {
                        dp[i][a][l + 1] = (dp[i][a][l + 1] + count) % mod
                    }
                }
            }
        }

        var result = 0L
        for (a in 0..1) {
            for (l in 0..2) {
                result = (result + dp[n][a][l]) % mod
            }
        }

        return result.toInt()
    }
}
```