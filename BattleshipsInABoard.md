# 419. Battleships in a Board

Given an `m x n` matrix `board` where each cell is a battleship `'X'` or empty `'.'`, return *the number of the **battleships** on `board`*.

**Battleships** can only be placed horizontally or vertically on `board`. In other words, they can only be made of the shape `1 x k` (`1` row, `k` columns) or `k x 1` (`k` rows, `1` column), where `k` can be of any size. At least one horizontal or vertical cell separates between two battleships (i.e., there are no adjacent battleships).

 

**Example 1:**
```
Input: board = [["X",".",".","X"],[".",".",".","X"],[".",".",".","X"]]
Output: 2
```

**Example 2:**
```
Input: board = [["."]]
Output: 0
```
 
**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 200`
- `board[i][j]` is either `'.'` or `'X'`.
 

**Follow up**: Could you do it in one-pass, using only $O(1)$ extra memory and without modifying the values `board`?

## Solution

**DFS**
```kotlin
class Solution {
    fun countBattleships(board: Array<CharArray>): Int {
        var sum = 0
        val m = board.size
        val n = board[0].size

        fun dfs(i: Int, j: Int) {
            if (board[i][j] == '.') return
            board[i][j] = '.'
            
            if (i + 1 < m && board[i + 1][j] == 'X') {
                dfs(i + 1, j)
            }

            if (i - 1 > 0 && board[i - 1][j] == 'X') {
                dfs(i - 1, j)
            }
            
            if (j + 1 < n  && board[i][j + 1] == 'X') {
                dfs(i, j + 1)
            }

            if (j - 1 > 0 && board[i][j - 1] == 'X') {
                dfs(i, j - 1)
            }
        }

        for (i in 0 until m) {
            for (j in 0 until n) {
                if (board[i][j] == 'X') {
                    dfs(i, j)
                    sum++
                }
            }
        }

        return sum
    }
}
```

**Follow up**

```kotlin
class Solution {
    fun countBattleships(board: Array<CharArray>): Int {
        var sum = 0
        val m = board.size
        val n = board[0].size

        for (i in 0 until m) {
            for (j in 0 until n) {
                if (
                    board[i][j] == 'X' && 
                    (i == 0 || board[i - 1][j] == '.') &&
                    (j == 0 || board[i][j - 1] == '.')
                  ) {
                    sum++
                }
            }
        }

        return sum
    }
}
```

Time complexity: O(m * n), extra space complexity O(1).