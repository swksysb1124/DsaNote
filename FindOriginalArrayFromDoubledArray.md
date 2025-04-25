# 2007. Find Original Array From Doubled Array

An integer array `original` is transformed into a **doubled** array `changed` by appending **twice the value** of every element in `original`, and then randomly **shuffling** the resulting array.

Given an array `changed`, return `original` *if `changed` is a **doubled** array. If `changed` is not a **doubled** array, return an empty array. The elements in `original` may be returned in **any** order*.

 

**Example 1:**
```
Input: changed = [1,3,4,2,6,8]
Output: [1,3,4]
Explanation: One possible original array could be [1,3,4]:
- Twice the value of 1 is 1 * 2 = 2.
- Twice the value of 3 is 3 * 2 = 6.
- Twice the value of 4 is 4 * 2 = 8.
Other original arrays could be [4,3,1] or [3,1,4].
```
**Example 2:**
```
Input: changed = [6,3,0,1]
Output: []
Explanation: changed is not a doubled array.
```
**Example 3:**
```
Input: changed = [1]
Output: []
Explanation: changed is not a doubled array.
``` 

**Constraints**:

- $1 <= changed.length <= 10^5$
- $0 <= changed[i] <= 10^5$

## Solution
```kotlin
class Solution {
    fun findOriginalArray(changed: IntArray): IntArray {
        // 數量不為整數，回傳空陣列
        if (changed.size % 2 != 0) return intArrayOf()

        // 使用 TreeMap 確保處理數字有小到大
        val count = TreeMap<Int, Int>()
        for (num in changed) {
            count[num] = (count[num] ?: 0) + 1
        }

        val res = mutableListOf<Int>()
        for ((num, freq) in count) {
            // 次數為零，可以之前就比對完了，跳過不處理
            if (freq == 0) continue

            if (num == 0) {
                // 特別處理 0 的情況，如果 0 的數目為奇數，回傳陣列
                // 如果為偶數，取一個數量放到 res 中
                if (freq % 2 != 0) return intArrayOf()
                repeat(freq / 2) { res.add(0) }
            } else {
                val double = num * 2
                val doubleFreq = count[double] ?: 0

                // doubleFreq 一定要大於或等於 freq
                if (doubleFreq < freq) return intArrayOf()

                // count[2*num] 減去 count[num]
                count[double] = doubleFreq - freq
                repeat(freq) { res.add(num) }
            }
        }

        return res.toIntArray()
    }
}
```