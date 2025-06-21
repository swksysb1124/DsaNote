# 55. Jump Game

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return *`true` if you can reach the last index, or `false` otherwise*.

**Example 1:**
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
**Example 2:**
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
``` 

**Constraints:**

- $1 <= nums.length <= 10^4$
- $0 <= nums[i] <= 10^5$

## Solution

直觀可以使用 recursive 方法，藉此找出轉移方程式，也可以使用備忘錄及DP解此問題

### Resursive
Top-Bottom
```kotlin
class Solution {
    fun canJump(nums: IntArray): Boolean {
        val n = nums.size
        // 判斷 start 為起點，是否能到達最後一格
        fun helper(start: Int): Boolean {
            // 起步在最後一格，回傳 true
            if (start == n - 1) return true

            for (step in 1..nums[start]) {
                // 不能超出範圍
                if (start + step < n) {
                    val canReached = helper(start + step)
                    // 如果發現可以到達最後一格，提前回傳 true
                    if (canReached) return true
                }
            }
            // 沒有一格可以抵達，回傳 false
            return false
        }
        
        return helper(0)
    }
}
```

### DP
Bottom-Up
```kotlin
class Solution {
    fun canJump(nums: IntArray): Boolean {
        val n = nums.size

        // dp[i]: 從 i 格是否可以到達最後一格
        val dp = BooleanArray(n) { false }
        // 最後一個位置為 true
        dp[n - 1] = true

        // 從倒數第二個格起步
        for (start in n - 2 downTo 0) {
            // 判斷是否可到達
            var canReached = false
            for (step in 1..nums[start]) {
                // 不能超出範圍 且 可到達最後一格
                if (start + step < n && dp[start + step]) {
                    canReached = true
                    break
                }
            }
            dp[start] = canReached
        }

        // 由出發位置判斷是否可以達到最後一格
        return dp[0]
    }
}
```

### 最佳解
```kotlin
class Solution {
    fun canJump(nums: IntArray): Boolean {
        val n = nums.size
        if (n == 1) return true

        // 倒數到二格開始往前
        var start = n - 2
        // 初始終點為最後一格
        var destination = n - 1

        while (start >= 0) {
            // 如果起點加上最大可移動步伐大於終點，
            // 表示更往前的起點到達目前起點也可以抵達最後一格
            if (start + nums[start] >= destination) {
                // 因此我們可以更新終點為目前起點
                destination = start
            }
            start--
        }

        // 判斷最後終點是不是在第一格
        return destination == 0
    }
}
```