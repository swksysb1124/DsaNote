# 1277. Count Square Submatrices with All Ones

Given a `m * n` matrix of ones and zeros, return how many **square** submatrices have all ones.


**Example 1:**
```
Input: matrix =
[
  [0,1,1,1],
  [1,1,1,1],
  [0,1,1,1]
]
Output: 15
Explanation: 
There are 10 squares of side 1.
There are 4 squares of side 2.
There is  1 square of side 3.
Total number of squares = 10 + 4 + 1 = 15.
```

**Example 2:**
```
Input: matrix = 
[
  [1,0,1],
  [1,1,0],
  [1,1,0]
]
Output: 7
Explanation: 
There are 6 squares of side 1.  
There is 1 square of side 2. 
Total number of squares = 6 + 1 = 7.
```

**Constraints:**

- `1 <= arr.length <= 300`
- `1 <= arr[0].length <= 300`
- `0 <= arr[i][j] <= 1`


## Solution

核心觀念：

轉移方程式為 

$f(i, j) = 1 + min(f(i + 1, j), f(i, j + 1), f(i + 1, j + 1))$

判斷一個 `(i, j)` 的最多可以數幾個正方形，可以右邊位置 `(i, j + 1)`、下面位置 `(i + 1, j)` 及右下位置 `(i + 1, j + 1)` 來判斷，取其最小再加 `1`。

**Top-Bottom recursive**

```kotlin
/**
 * Top-Bottom 掃描方向: 由上而下，從左到右
 *
 * |------> |
 * |---->   |
 * |-->     |
 * |->      |
 */

class Solution {
    fun countSquares(matrix: Array<IntArray>): Int {
        val m = matrix.size
        val n = matrix[0].size

        fun dfs(i: Int, j: Int): Int {
            if (i == m || j == n || matrix[i][j] == 0) {
                return 0
            }

            var count = min(dfs(i + 1, j), dfs(i, j + 1))
            count = 1 + min(count, dfs(i + 1, j + 1))
            return count
        }

        var res = 0
        for (i in 0..<m) {
            for (j in 0..<n) {
                if (matrix[i][j] == 1) {
                    res += dfs(i, j)
                }
            }
        }
        return res
    }
}
```

**Bottom-Up DP**

```kotlin

/**
 * Bottom-Up 掃描方向: 由下而上，從右到左
 * 
 * |      <-|
 * |     <--|
 * |   <----|
 * | <------|
 */


class Solution {
    fun countSquares(matrix: Array<IntArray>): Int {
        val m = matrix.size
        val n = matrix[0].size

        val dp = Array(m + 1) { IntArray(n + 1) { 0 } }

        var res = 0

        for (i in m - 1 downTo 0) {
            for (j in n - 1 downTo 0) {
                if (matrix[i][j] == 1) {
                    var min = min(dp[i + 1][j], dp[i][j + 1])
                    min = min(min, dp[i + 1][j + 1]) 
                    dp[i][j] = min + 1
                    res += dp[i][j]
                }
            }
        }

        return res
    }
}
```

