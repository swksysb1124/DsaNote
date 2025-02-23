# 51. N-Queens

The n-queens puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return all distinct solutions to the **n-queens puzzle**. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

```
Input: n = 4
Output: [[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above
```

**Example 2:**

```
Input: n = 1
Output: [["Q"]]
```

**Constraints:**

- `1 <= n <= 9`

## Solution

### backtracking

```kotlin
class Solution {
    fun solveNQueens(n: Int): List<List<String>> {
        val res = mutableListOf<List<String>>()
        val board = Array(n) { CharArray(n) { '.' } }

        fun backtracking(i: Int) {
            if (i == n) {
                val newLine = mutableListOf<String>()
                board.forEach { row ->
                    newLine.add(String(row))
                }
                res.add(newLine)
                return
            }

            for (j in 0 until n) {
                if (!isValid(i, j, board)) continue
                board[i][j] = 'Q'
                backtracking(i + 1)
                board[i][j] = '.'
            }
        }

        backtracking(0)

        return res
    }

    private fun isValid(i: Int, j: Int, board: Array<CharArray>): Boolean {
        val n = board.size

        // 同 col 是否有 Q
        for (row in 0..i-1) {
            if(board[row][j] == 'Q') return false
        }

        // 右上是否有 Q
        var row = i - 1
        var col = j + 1
        while (row >= 0 && col < n) {
            if (board[row][col] == 'Q') return false
            row--
            col++
        }

        // 左上是否呦 Q
        row = i - 1
        col = j - 1
        while (row >= 0 && col >= 0) {
            if (board[row][col] == 'Q') return false
            row--
            col--
        }

        return true
    }
}
```