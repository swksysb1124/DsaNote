# 322. Coin Change

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the *fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.


**Example 1:**
```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**
```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**
```
Input: coins = [1], amount = 0
Output: 0
```
 
**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 2^31 - 1`
- `0 <= amount <= 10^4`

## Solution
### 暴力解法

```kotlin
class Solution {
    fun coinChange(coins: IntArray, amount: Int): Int {
        // base case
        if (amount == 0) return 0

        var res = Int.MAX_VALUE
        for (coin in coins) {
            // 硬幣幣值大於金額，跳過
            if (coin > amount) continue

            val subquestion = coinChange(coins, amount - coin)

            // 剩餘金額無解(子問題無解），跳過
            if (subquestion == -1) continue

            // 比較所有 subquestion + 1 最小值。
            // 加 1 是加上一開始的硬幣
            res = min(res, subquestion + 1)
        }

        return if (res != Int.MAX_VALUE) res else -1
    }
}
```

### 備忘錄解法

```kotlin
class Solution {

    fun coinChange(coins: IntArray, amount: Int): Int {
        val memo = IntArray(amount + 1) { Int.MAX_VALUE }

        fun helper(coins: IntArray, n: Int) {
            // 如果備忘錄有紀錄，直接返回
            if (memo[n] != Int.MAX_VALUE) return memo[n]

            if (n == 0) return 0

            var res = Int.MAX_VALUE
            for (coin in coins) {
                // 硬幣幣值大於金額，跳過
                if (coin > n) continue

                val subquestion = helper(coins, n - coin)

                // 剩餘金額無解(子問題無解），跳過
                if (subquestion == -1) continue
    
                // 比較所有 subquestion + 1 最小值。
                // 加 1 是加上一開始的硬幣
                res = min(res, subquestion + 1)
            }

            // 更新備忘錄
            memo[n] = if (res != Int.MAX_VALUE) res else -1
            return memo[n]
        }

        return helper(coins, amount)
    }
}
```

### dp

```kotlin
class Solution {
    fun coinChange(coins: IntArray, amount: Int): Int {
        // dp[n] 表示金額為 n 所需的最少硬幣數
        val dp = IntArray(amonut + 1) { -1 }

        // base case
        dp[0] = 0

        for (i in 1..amount) {
            var res = Int.MAX_VALUE
            for (coin in coins) {
                // 硬幣幣值大於金額，跳過
                if (coin > i) continue

                // 剩餘金額無解(子問題無解），跳過
                if (dp[i - coin] == -1) return

                // 比較所有 subquestion + 1 最小值。
                // 加 1 是加上一開始的硬幣
                res = min(res, 1 + dp[i - coin])
            }
            dp[i] = if (res != Int.MAX_VALUE) res else -1
        }

        return dp[amount]
    }
}
```
