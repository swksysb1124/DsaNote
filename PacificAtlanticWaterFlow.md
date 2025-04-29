# 417. Pacific Atlantic Water Flow

There is an `m x n` rectangular island that borders both the **Pacific Ocean** and **Atlantic Ocean**. The **Pacific Ocean** touches the island's left and top edges, and the **Atlantic Ocean** touches the island's right and bottom edges.

The island is partitioned into a grid of square cells. You are given an `m x n` integer matrix `heights` where `heights[r][c]` represents the height above sea level of the cell at coordinate `(r, c)`.

The island receives a lot of rain, and the rain water can flow to neighboring cells directly north, south, east, and west if the neighboring cell's height is less than or equal to the current cell's height. Water can flow from any cell adjacent to an ocean into the ocean.

Return *a **2D** list of grid coordinates `result` where $result[i] = [r_i, c_i]$ denotes that rain water can flow from cell $(r_i, c_i)$ to both the Pacific and Atlantic oceans*.


**Example 1:**
```
Input: heights = [[1,2,2,3,5],[3,2,3,4,4],[2,4,5,3,1],[6,7,1,4,5],[5,1,1,2,4]]
Output: [[0,4],[1,3],[1,4],[2,2],[3,0],[3,1],[4,0]]
Explanation: The following cells can flow to the Pacific and Atlantic oceans, as shown below:
[0,4]: [0,4] -> Pacific Ocean 
       [0,4] -> Atlantic Ocean
[1,3]: [1,3] -> [0,3] -> Pacific Ocean 
       [1,3] -> [1,4] -> Atlantic Ocean
[1,4]: [1,4] -> [1,3] -> [0,3] -> Pacific Ocean 
       [1,4] -> Atlantic Ocean
[2,2]: [2,2] -> [1,2] -> [0,2] -> Pacific Ocean 
       [2,2] -> [2,3] -> [2,4] -> Atlantic Ocean
[3,0]: [3,0] -> Pacific Ocean 
       [3,0] -> [4,0] -> Atlantic Ocean
[3,1]: [3,1] -> [3,0] -> Pacific Ocean 
       [3,1] -> [4,1] -> Atlantic Ocean
[4,0]: [4,0] -> Pacific Ocean 
       [4,0] -> Atlantic Ocean
Note that there are other possible paths for these cells to flow to the Pacific and Atlantic oceans.
```

**Example 2:**
```
Input: heights = [[1]]
Output: [[0,0]]
Explanation: The water can flow from the only cell to the Pacific and Atlantic oceans.
``` 

**Constraints:**

- `m == heights.length`
- `n == heights[r].length`
- `1 <= m, n <= 200`
- $0 <= heights[r][c] <= 10^5$


## Solution

**核心觀念**： 分別由從 **太平洋海岸邊(上/左邊緣)**、**大西洋海岸邊(右/下邊緣)** 進行 **DFS**，反向紀錄可以到達該海洋的節點。**注意是反向，所以前一個節點的高度要低於後一個節點高度**。最後從可到達大平洋及大西洋的紀錄節點中，取可能都可以到達的節點。


```kotlin
class Solution {
    fun pacificAtlantic(heights: Array<IntArray>): List<List<Int>> {
        if (heights.isEmpty()) return emptyList()
        val m = heights.size
        val n = heights[0].size
        val pacificReached = Array(m) { BooleanArray(n) { false } }
        val atlanticReached = Array(m) { BooleanArray(n) { false } }

        for (i in 0 until m) {
            dfs(heights, pacificReached, i, 0, -1)
            dfs(heights, atlanticReached, i, n - 1, -1)
        }

        for (j in 0 until n) {
            dfs(heights, pacificReached, 0, j, -1)
            dfs(heights, atlanticReached, m - 1, j, -1)
        }

        val res = mutableListOf<List<Int>>()

        for (i in 0 until m) {
            for (j in 0 until n) {
                if (pacificReached[i][j] && atlanticReached[i][j]) {
                    res.add(listOf(i, j))
                }
            }
        }
        return res
    }

    private fun dfs(heights: Array<IntArray>, visited: Array<BooleanArray>, i: Int, j: Int, preH: Int) {
        val m = heights.size
        val n = heights[0].size
        if (visited[i][j] || preH > heights[i][j]) return
        visited[i][j] = true
        if (i + 1 < m) dfs(heights, visited, i + 1, j, heights[i][j])
        if (i - 1 >= 0) dfs(heights, visited, i - 1, j, heights[i][j])
        if (j + 1 < n) dfs(heights, visited, i, j + 1, heights[i][j])
        if (j - 1 >= 0) dfs(heights, visited, i, j - 1, heights[i][j])
    }
}
```