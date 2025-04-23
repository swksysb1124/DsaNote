# 62. Unique Paths

There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the **bottom-right corner** (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return t*he number of possible unique paths that the robot can take to reach the bottom-right corner*.

The test cases are generated so that the answer will be less than or equal to $2 * 10^9$.


**Example 1:**
```
Input: m = 3, n = 7
Output: 28
```
**Example 2:**
```
Input: m = 3, n = 2
Output: 3
Explanation: From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down
```

**Constraints:**

- `1 <= m, n <= 100`

## Solution

**遞迴**
```kotlin
class Solution {
    fun uniquePaths(m: Int, n: Int): Int {
        // 由終點數回來
        return helper(m - 1, n - 1)
    }

    private fun helper(i: Int, j: Int): Int {
        // 如果 i, j 之中有小於 0, 表示超出界外，沒有路徑
        if (i < 0 || j < 0) return 0

        // 如果 i, j 有等於零，表示在第一列 或 第一行，只會有一個方向的路徑
        if (i == 0 || j == 0) return 1

        // 路徑是由上方路徑和 加總 左邊路徑和
        return helper(i - 1, j) + helper(i, j - 1)
    }
}
```

**DP**

核心概念：

- 用 `dp[i][j]` 來表示由 `(0, 0)` 位置到 `(i, j)` 位置的路徑數目
- 當走在第一列 (row)，路徑只有一個，也就是**由左到右**的方向，因此 `dp[0][j] = 1, j in [0, n)`。
- 當走在第一行 (column)，路徑只有一個，也就是**由上到下**的方向，因此 `dp[i][0] = 1, i in [0, m)`。
- 當 `i in [1, m)` 且 `j in [1, n)`，`dp[i][j] = dp[i-1][j] + dp[i][j-1]`。其中 `dp[i-1][j]` 上方位置的路徑數目，`dp[i][j-1]` 左邊位置的路徑數目。

```kotlin
class Solution {
    fun uniquePaths(m: Int, n: Int): Int {
        val dp = Array(m) { IntArray(n) { 0 }}
        for (i in 0..<m) { dp[i][0] = 1 }
        for (j in 0..<n) { dp[0][j] = 1 }
        for (i in 1..<m) {
            for (j in 1..<n) {
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
            }
        }
        return dp[m-1][n-1]
    }
}
```

壓縮到一維

```kotlin
class Solution {
    fun uniquePaths(m: Int, n: Int): Int {
        val dp = IntArray(n) { 0 }
        dp[0] = 1

        for (i in 0..<m) {
            for (j in 1..<n) {
                dp[j] = dp[j] + dp[j - 1]
            }
        }
        
        return dp[n-1]
    }
}
```
