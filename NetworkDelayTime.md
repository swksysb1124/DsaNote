# 743. Network Delay Time

You are given a network of `n` nodes, labeled from `1` to `n`. You are also given times, a list of travel `times` as directed edges $times[i] = (u_i, v_i, w_i)$, where $u_i$ is the source node, $v_i$ is the target node, and $w_i$ is the time it takes for a signal to travel from source to target.

We will send a signal from a given node `k`. Return *the **minimum** time it takes for all the `n` nodes to receive the signal*. If it is impossible for all the `n` nodes to receive the signal, return `-1`.

 

**Example 1:**

```
Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
Output: 2
```

**Example 2:**
```
Input: times = [[1,2,1]], n = 2, k = 1
Output: 1
```

**Example 3:**
```
Input: times = [[1,2,1]], n = 2, k = 2
Output: -1
```
 

**Constraints:**

- `1 <= k <= n <= 100`
- `1 <= times.length <= 6000`
- `times[i].length == 3`
- $1 <= u_i, v_i <= n$
- $u_i != v_i$
- $0 <= w_i <= 100$
- All the pairs $(u_i, v_i)$ are **unique**. (i.e., no multiple edges.)


## Solution
```kotlin
class Solution {
    fun networkDelayTime(times: Array<IntArray>, n: Int, k: Int): Int {
        val INT = Int.MAX_VALUE

        // initialize graph
        val graph = Array(n) { mutableListOf<Pair<Int, Int>>() } // u: [v, w]
        for ((u, v, w) in times) {
            graph[u - 1].add((v - 1) to w) // u - 1 & v - 1 for 0-indexed
        }

        // init distances
        val distances = Array(n) { INT }
        distances[k - 1] = 0 // k - 1 for 0-indexed

        val minHeap = PriorityQueue<Pair<Int, Int>> { a, b -> 
            a.second.compareTo(b.second) // sorting by w 
        }
        minHeap.add(k - 1 to 0)

        while (minHeap.isNotEmpty()) {
            val (u, currDist) = minHeap.poll()

            // if currDist is not the min path, ignore it
            if (currDist > distances[u]) continue

            for ((v, w) in graph[u]) {
                val newDist = currDist + w
                if (newDist < distances[v]) {
                    distances[v] = newDist
                    minHeap.add(v to newDist)
                }
            }
        }

        return distances.maxOrNull()?.takeIf { it != INT } ?: -1
    }
}
```