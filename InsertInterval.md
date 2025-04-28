# 57. Insert Interval

You are given an array of non-overlapping intervals `intervals` where $intervals[i] = [start_i, end_i]$ represent the start and the end of the $i_th$ interval and `intervals` is sorted in ascending order by $start_i$. You are also given an interval `newInterval = [start, end]` that represents the start and end of another interval.

Insert `newInterval` into `intervals` such that `intervals` is still sorted in ascending order by $start_i$ and `intervals` still does not have any overlapping intervals (merge overlapping intervals if necessary).

Return `intervals` after the insertion.

**Note** that you don't need to modify `intervals` in-place. You can make a new array and return it.

**Example 1:**
```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```

**Example 2:**
```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
``` 

**Constraints:**

- $0 <= intervals.length <= 10^4$
- $intervals[i].length == 2$
- $0 <= start_i <= end_i <= 10^5$
- `intervals` is sorted by $start_i$ in **ascending** order.
- `newInterval.length == 2`
- $0 <= start <= end <= 10^5$

## Solution

**核心概念：**

一筆一筆比對 `intervals` 中的 `interval` 跟 `newInterval` 的關係，並依在以下資訊更新 `res`：
- 當**目前區間**在**新區間之前** (`interval[1] < newInterval[0]`)，可以直接放到 `res` 中。
  
- 當表示**目前區間及接下來區間們**都在**新區間之後** (`interval[0] > newInterval[1]`)，可以依序放入 `newInterval` 跟接下來區間們，並返回 `res`。

- 其他的情況，表示 `interval` 跟 `newInterval` 有重疊，將 `newInterval` 更新為**兩區間合併**，繼續接下來的比對。

- 在沒有新區間之後的情況，最後必須把 `newInterval` 加入 `res`，並返回 `res`。

```kotlin
class Solution {
    fun insert(intervals: Array<IntArray>, newInterval: IntArray): Array<IntArray> {
        val res = mutableListOf<IntArray>()
        val n = intervals.size
        var insert = newInterval
        for ((i, interval) in intervals.withIndex()) {
            if (interval[1] < insert[0]) {
                res.add(interval)
            } else if (interval[0] > insert[1]) {
                res.add(insert)
                for (j in i until n) {
                    res.add(intervals[j])
                }
                return res.toTypedArray()
            } else {
                insert = intArrayOf(
                    min(interval[0], insert[0]),
                    max(interval[1], insert[1])
                )
            }
        }

        res.add(insert)
        return res.toTypedArray()
    }
}
```