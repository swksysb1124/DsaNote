# 752. Open the Lock

You have a lock in front of you with 4 circular wheels. Each wheel has 10 slots: `'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'`. The wheels can rotate freely and wrap around: for example we can turn '9' to be `'0'`, or `'0'` to be `'9'`. Each move consists of turning one wheel one slot.

The lock initially starts at `'0000'`, a string representing the state of the 4 wheels.

You are given a list of `deadends` dead ends, meaning if the lock displays any of these codes, the wheels of the lock will stop turning and you will be unable to open it.

Given a `target` representing the value of the wheels that will unlock the lock, return the minimum total number of turns required to open the lock, or `-1` if it is impossible.


**Example 1:**
```
Input: deadends = ["0201","0101","0102","1212","2002"], target = "0202"
Output: 6
Explanation: 
A sequence of valid moves would be "0000" -> "1000" -> "1100" -> "1200" -> "1201" -> "1202" -> "0202".
Note that a sequence like "0000" -> "0001" -> "0002" -> "0102" -> "0202" would be invalid,
because the wheels of the lock become stuck after the display becomes the dead end "0102".
```

**Example 2:**
```
Input: deadends = ["8888"], target = "0009"
Output: 1
Explanation: We can turn the last wheel in reverse to move from "0000" -> "0009".
```
**Example 3:**
```
Input: deadends = ["8887","8889","8878","8898","8788","8988","7888","9888"], target = "8888"
Output: -1
Explanation: We cannot reach the target without getting stuck.
 ```

**Constraints:**

- `1 <= deadends.length <= 500`
- `deadends[i].length == 4`
- `target.length == 4`
- target will not be in the list `deadends`.
- `target` and `deadends[i]` consist of digits only.

## Solution

### BFS

```kotlin
class Solution {
    fun openLock(deadends: Array<String>, target: String): Int {
        val queue = LinkedList<String>()
        // 記錄死亡數字
        val dead = deadends.toSet()
        // 計錄使用過密碼
        val visited = mutableSetOf<String>()
        queue.offer("0000")

        var step = 0

        while (queue.isNotEmpty()) {
            val size = queue.size

            for (i in 0 until size) {
                // 判斷是否符合條件
                val curr = queue.poll()
                if (dead.contains(curr)) continue
                if (curr == target) {
                    return step
                }

                // 加入 curr 下一層節點
                for (j in 0 until 4) {
                    val plus = plusOne(j, curr)
                    if (!dead.contains(plus) && !visited.contains(plus)) {
                        queue.offer(plus)
                        visited.add(plus)
                    }

                    val minus = minusOne(j, curr)
                    if (!dead.contains(minus) && !visited.contains(minus)) {
                        queue.offer(minus)
                        visited.add(minus)
                    }
                }
            }
            step++
        }
        return if (visited.contains(target)) step else -1
    }

    private fun plusOne(pos: Int, curr: String): String {
        val ca = curr.toCharArray()
        if (ca[pos] == '9') {
            ca[pos] = '0'
        } else {
            ca[pos] = (ca[pos].toInt() + 1).toChar()
        }
        return String(ca)
    }

    private fun minusOne(pos: Int, curr: String): String {
        val ca = curr.toCharArray()
        if (ca[pos] == '0') {
            ca[pos] = '9'
        } else {
            ca[pos] = (ca[pos].toInt() - 1).toChar()
        }
        return String(ca)
    }
}
```