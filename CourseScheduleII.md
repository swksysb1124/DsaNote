# 210. Course Schedule II

There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where $prerequisites[i] = [a_i, b_i]$ indicates that you **must** take course $b_i$ first if you want to take course $a_i$.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.

Return *the ordering of courses you should take to finish all courses*. If there are many valid answers, return    of them. If it is impossible to finish all courses, return **an empty array**.

 

**Example 1:**
```
Input: numCourses = 2, prerequisites = [[1,0]]
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].
```
**Example 2:**
```
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]
Output: [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].
```
**Example 3:**
```
Input: numCourses = 1, prerequisites = []
Output: [0]
``` 

Constraints:

- `1 <= numCourses <= 2000`
- `0 <= prerequisites.length <= numCourses * (numCourses - 1)`
- `prerequisites[i].length == 2`
- $0 <= a_i, b_i < numCourses$
- $a_i != b_i$
- All the pairs $[a_i, b_i]$ are **distinct**.

## Solution

核心觀念：

採用 Topological Sort 演算法來實作。Topological Sort 的實作內容為：
- 產生一個 preGraph 紀錄課程跟他們先修課關係
- 每一門課都採用 DFS 來遍歷其先修課程
- 記錄 visit 以免重複計算路徑，發現有經過的課程，不用繼續遍歷。
- 每次便利路徑開始記錄 cycle，一但發現有產生環形路徑，立刻返回錯誤。

```kotlin
class Solution {
    fun findOrder(numCourses: Int, prerequisites: Array<IntArray>): IntArray {
        val preGraph = Array(numCourses) { mutableListOf<Int>() }
        for (pre in prerequisites) {
            preGraph[pre[0]].add(pre[1])
        }
        val visit = HashSet<Int>()
        val cycle = HashSet<Int>()
        val res = mutableListOf<Int>()

        fun dfs(course: Int): Boolean {
            if (cycle.contains(course)) return false
            if (visit.contains(course)) return true
            cycle.add(course)
            for (c in preGraph[course]) {
                if (!dfs(c)) return false
            }
            cycle.remove(course)
            visit.add(course)
            res.add(course)
            return true
        }

        for (c in 0 until numCourses) {
            if (!dfs(c)) return IntArray(0)
        }
        return res.toIntArray()
    }
}
```