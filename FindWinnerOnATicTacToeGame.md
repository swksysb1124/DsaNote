# 1275. Find Winner on a Tic Tac Toe Game

**Tic-tac-toe** is played by two players A and B on a `3 x 3` grid. The rules of Tic-Tac-Toe are:

- Players take turns placing characters into empty squares `' '`.
  
- The first player A always places `'X'` characters, while the second player B always places `'O'` characters.

- `'X'` and `'O'` characters are always placed into empty squares, never on filled ones.

- The game ends when there are **three** of the same (non-empty) character filling any row, column, or diagonal.

- The game also ends if all squares are non-empty.

- No more moves can be played if the game is over.

Given a 2D integer array moves where $moves[i] = [row_i, col_i]$ indicates that the $i^th$ move will be played on $grid[row_i][col_i]$. return *the winner of the game if it exists* (`A` or `B`). In case the game ends in a draw return `"Draw"`. If there are still movements to play return `"Pending"`.

You can assume that `moves` is valid (i.e., it follows the rules of **Tic-Tac-Toe**), the grid is initially empty, and `A` will play first.

**Example 1:**
```
Input: moves = [[0,0],[2,0],[1,1],[2,1],[2,2]]
Output: "A"
Explanation: A wins, they always play first.
```

**Example 2:**
```
Input: moves = [[0,0],[1,1],[0,1],[0,2],[1,0],[2,0]]
Output: "B"
Explanation: B wins.
```

**Example 3:**
```
Input: moves = [[0,0],[1,1],[2,0],[1,0],[1,2],[2,1],[0,1],[0,2],[2,2]]
Output: "Draw"
Explanation: The game ends in a draw since there are no moves to make.
``` 

**Constraints:**

- `1 <= moves.length <= 9`
- `moves[i].length == 2`
- $0 <= row_i, col_i<= 2$
- There are no repeated elements on `moves`.
- `moves` follow the rules of tic tac toe.

## Solution
```kotlin
class Solution {
    fun tictactoe(moves: Array<IntArray>): String {
        // 0: no placement
        // 1: placed by first player
        // 2: placed by first player
        val game = Array(3) { IntArray(3) { 0 } }

        // initialize Tic-Tac-Toe game
        for ((i, move) in moves.withIndex()) {
            game[move[0]][move[1]] = if (i % 2 == 0) 1 else 2
        }

        // check horizontal lines if there is a winner
        for (i in 0..2) {
            if (game[i][0] == game[i][1] && game[i][1] == game[i][2]) {
                when (game[i][0]) {
                    1 -> return "A"
                    2 -> return "B"
                }
            }
        }

        // check vertical lines if there is a winner
        for (j in 0..2) {
            if (game[0][j] == game[1][j] && game[1][j] == game[2][j]) {
                when (game[0][j]) {
                    1 -> return "A"
                    2 -> return "B"
                }
            }
        }

        // check the diagonal line if there is a winner
        if (game[0][0] == game[1][1] && game[1][1] == game[2][2]) {
            when (game[0][0]) {
                1 -> return "A"
                2 -> return "B"
            }
        }

        // check the diagonal line if there is a winner
        if (game[0][2] == game[1][1] && game[1][1] == game[2][0]) {
            when (game[0][2]) {
                1 -> return "A"
                2 -> return "B"
            }
        }

        // if there are more than nice moves without winner
        // return Draw
        if (moves.size >= 9) {
            return "Draw"
        }

        return "Pending"   
    }
}
```