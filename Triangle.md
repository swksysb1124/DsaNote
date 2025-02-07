# 120. Triangle

Given a `triangle` array, return the minimum path sum from top to bottom.

For each step, you may move to an adjacent number of the row below. More formally, if you are on index i on the current row, you may move to either index `i` or index `i + 1` on the next row.

**Example 1:**
```
Input: triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
Output: 11
Explanation: The triangle looks like:
   2
  3 4
 6 5 7
4 1 8 3
The minimum path sum from top to bottom is 2 + 3 + 5 + 1 = 11 (underlined above).
```

**Example 2:**
```
Input: triangle = [[-10]]
Output: -10
```

**Constraints:**

- `1 <= triangle.length <= 200`
- `triangle[0].length == 1`
- `triangle[i].length == triangle[i - 1].length + 1`
- `-10^4 <= triangle[i][j] <= 10^4`
 

Follow up: Could you do this using only `O(n)` extra space, where n is the total number of rows in the triangle?

**DP Solution:**
```kotlin
class Solution {
    // n = 4
    // [2] -- 0
    // [3][4] -- 1
    // [5][6][7] -- 2
    // [4][1][8][3] -- 3
    fun minimumTotal(triangle: List<List<Int>>): Int {
        val n = triangle.size
        if (n == 1) return triangle[0][0]

        // dp[i] : the min path sum from position i to the end
        // copy the last level to init the dp
        val dp = triangle[n-1].toMutableList()

        // from the last second level 
        for (level in n - 2 downTo 0) { 
            for (j in 0..level) {
                dp[j] = min(dp[j], dp[j+1]) + triangle[level][j]
            }
        }
        return dp[0]
    }
}
```
