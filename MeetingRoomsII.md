# 253. Meeting Rooms II

Given an array of meeting time `intervals` consisting of **start** and **end** times `[[s1,e1],[s2,e2],...]` `(si < ei)`, find *the minimum number of conference rooms required*.

For example, Given `[[0, 30],[5, 10],[15, 20]]`, return `2`.

## Solution

### Min Heap
說明

- 按照會議**開始時間**排序，並用一個 **min heap** 來追蹤目前所有會議的**結束時間**。
- 如果新的會議的開始時間 ≥ heap 最早結束時間 ⇒ 可以重用會議室 ⇒ 拿掉那個結束時間（poll()）
- 無論能否重用，都要把目前會議的結束時間加入 heap。
- 最後 heap 的 size 就代表目前正在進行的會議數，也就是當下需要的會議室數。

```kotlin
class Solution {
    fun minMeetingRooms(intervals: Array<IntArray>): Int {
        // 依照開始時間排序 intervals
        intervals.sortWith { a, b->
            a[0].compareTo(b[0])
        }

        // 生成一個 min heap 來記錄最小結束時間
        val priorityQueue = PriorityQueue<Int>()

        // 將第一個 interval 的結束時間放到 min heap
        priorityQueue.add(intervals[0][1])

        for (i in 1..<intervals.size) {
            val interval = intervals[i]

            // 如果 當前時間區間的結束時間大於或等於 目前最小結束時間
            // 表示會議室可以重複使用，可以除掉 目前最小結束時間
            if (interval[0] >= priorityQueue.peek()) {
                priorityQueue.poll()
            }
            // 將當前時間區間的結束時間 放入 min heap
            priorityQueue.add(interval[1])
        }
        return priorityQueue.size
    }
}
```

### Scanning line

把所有時間點拆成：
- 一個會議的「開始時間」：+1（需要新會議室）
- 一個會議的「結束時間」：-1（釋出一個會議室）

然後依照時間排序後掃過一次，累積當下需要的會議室數量，取最大值即可。

```kotlin
class Solution {
    fun minMeetingRooms(intervals: Array<IntArray>): Int {
        val times = mutableList<Pair<Integer, Integer>>() // (時間, 變量)

        for (interval in intervals) {
            time.add(Pair(interval[0], 1))  // 開始 +1
            time.add(Pair(interval[1], -1)) // 結束 -1
        }

        // 先排時間，再排變量 -1 -> 1
        times.sortWith{
            compareBy({ it.first }, { it.second })
        }

        var rooms = 0
        var max = 0

        for ((_, delta) in times) {
            rooms += delta
            max = max(max, rooms)
        }
        return max
    }
}
```