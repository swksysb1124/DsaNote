# 200. Number of Islands

Given an `m` x `n` 2D binary grid `grid` which represents a map of `'1's` (land) and `'0's` (water), return *the number of islands*.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

 

**Example 1:**
```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```
**Example 2:**
```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
``` 

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

## Solution
```kotlin
class Solution {
    fun numIslands(grid: Array<CharArray>): Int {
        val m = grid.size
        val n = grid[0].size
        var islandNum = 0
        for (i in 0 until m) {
            for (j in 0 until n) {
                if (grid[i][j] == '1') {
                    dfs(i, j, grid)
                    islandNum++
                }
            }
        }
        return islandNum
    }

    fun dfs(i: Int, j: Int, grid: Array<CharArray>) {
        if (grid[i][j] == '0') return
        // mark as water to avoid looping
        grid[i][j] = '0'
        if (i + 1 < grid.size) dfs(i + 1, j, grid)
        if (i - 1 >= 0) dfs(i - 1, j, grid)
        if (j + 1 < grid[0].size) dfs(i, j + 1, grid)
        if (j - 1 >= 0) dfs(i, j - 1, grid)
    }
}
```