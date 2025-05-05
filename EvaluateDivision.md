# 399. Evaluate Division

You are given an array of variable pairs `equations` and an array of real numbers `values`, where <code>equations[i] = [A<sub>i</sub>, B<sub>i</sub>]</code> and `values[i]` represent the equation <code>A<sub>i</sub> / B<sub>i</sub> = values[i]</code>. Each <code>A<sub>i</sub></code> or <code>B<sub>i</sub></code> is a string that represents a single variable.

You are also given some queries, where <code>queries[j] = [C<sub>j</sub>, D<sub>j</sub>]</code> represents the jth query where you must find the answer for <code>C<sub>j</sub> / D<sub>j</sub> = ?</code>.

Return *the answers to all queries. If a single answer cannot be determined, return `-1.0`*.

**Note**: The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.

**Note**: The variables that do not occur in the list of equations are undefined, so the answer cannot be determined for them.

 

**Example 1:**
```
Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
Explanation: 
Given: a / b = 2.0, b / c = 3.0
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? 
return: [6.0, 0.5, -1.0, 1.0, -1.0 ]
note: x is undefined => -1.0
```
**Example 2:**
```
Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
Output: [3.75000,0.40000,5.00000,0.20000]
```
**Example 3:**
```
Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
Output: [0.50000,2.00000,-1.00000,-1.00000]
```
 

**Constraints:**

- `1 <= equations.length <= 20`
- `equations[i].length == 2`
- `1 <= Ai.length, Bi.length <= 5`
- `values.length == equations.length`
- `0.0 < values[i] <= 20.0`
- `1 <= queries.length <= 20`
- `queries[i].length == 2`
- `1 <= Cj.length, Dj.length <= 5`
- `Ai`, `Bi`, `Cj`, `Dj` consist of lower case - English letters and digits.

## Solution
```kotlin
class Solution {
    fun calcEquation(equations: List<List<String>>, values: DoubleArray, queries: List<List<String>>): DoubleArray {
        val graph = HashMap<String, HashMap<String, Double>>()

        val n = equations.size
        for (i in 0..<n) {
            val (x, y) = equations[i]
            val value = values[i]
            graph.putIfAbsent(x, HashMap<String, Double>())
            graph.putIfAbsent(y, HashMap<String, Double>())
            graph[x]?.set(y, value)
            graph[y]?.set(x, 1 / value)
        }

        // 使用 visited 紀錄起點，避免尋找路線，出現迴路
        val visited = HashSet<String>()

        fun dfs(x: String, y: String): Double {
            // 如果 x, y 沒有在 graph 中，沒有路徑
            if (graph[x] == null || graph[y] == null) return -1.0
            // 如果 x, y 相等，直接回傳 1
            if (x == y) return 1.0
            // 如果問的就是 graph 存在路徑，直接返回
            if (graph[x]?.get(y) != null) return graph[x]?.get(y) ?: -1.0


            visited.add(x)
            for (mid in graph[x]?.keys.orEmpty()) {
                if (!visited.contains(mid)) {
                    val factor = dfs(mid, y)
                    if (factor != -1.0) {
                        return graph[x]?.get(mid)!! * factor
                    }
                }
            }

            return -1.0
        }

        val m = queries.size
        val res = DoubleArray(m)
        
        for (i in 0..<m) {
            val (x, y) = queries[i]
            // 清空 visited 避免影響運算
            visited.clear()
            res[i] = dfs(x, y)
        }

        return res
    }
}
```