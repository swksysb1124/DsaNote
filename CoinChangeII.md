# 518. Coin Change II

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer. 

**Example 1:**

```
Input: amount = 5, coins = [1,2,5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**Example 2:**
```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```
**Example 3:**
```
Input: amount = 10, coins = [10]
Output: 1
```

**Constraints:**
- `1 <= coins.length <= 300`
- `1 <= coins[i] <= 5000`
- All the values of `coins` are unique.
- `0 <= amount <= 5000`

## Solution

這是一種**完全背包問題 (Unbounded Knapsack Problem)**。

**2D 動態規劃**

```kotlin
class Solution {
    fun change(amount: Int, coins: IntArray): Int {
        val n = coins.size

        // dp[i][j]: 當包含到第 i coin 時，總數 j，可有 dp[i][j] 種組合放室
        val dp = Array(n + 1) {
            IntArray(amount + 1) { 0 }
        }

        // dp[..][0]: 當總數為來只有一種組合方式
        for (i in 0..n) { dp[i][0] = 1 }

        for (i in 1..n) {
            for (j in 1..amount) {
                if (j >= coins[i - 1]) {
                    // 放入 第 i 個 coins + 不放入 第 i 個 coins
                    dp[i][j] = dp[i][j - coins[i - 1]] + dp[i - 1][j]
                } else {
                    // 不放入 第 i 個 coins
                    dp[i][j] = dp[i - 1][j]
                }
            }
        }

        return dp[n][amount]
    }
}
```

**1D 動態規劃**

```kotlin
class Solution {
    fun change(amount: Int, coins: IntArray): Int {
        val n = coins.size

        // dp[j]: 總數為 j, 可以有幾種組合 
        val dp = IntArray(amount + 1) { 0 }

        dp[0] = 1

        for (i in 0 until n) {
            for (j in 1..amount) {
                if (j >= coins[i]) {
                    // 放入 第 i 個 coins + 不放入 第 i 個 coins
                    dp[j] = dp[j - coins[i]] + dp[j]
                }
            }
        }

        return dp[amount]
    }
}
```

