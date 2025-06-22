# 45. Jump Game II

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at `nums[0]`.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at `nums[i]`, you can jump to any `nums[i + j]` where:

- `0 <= j <= nums[i]` and

- `i + j < n`

Return *the minimum number of jumps to reach `nums[n - 1]`. The test cases are generated such that you can reach `nums[n - 1]`*.

**Example 1:**
```
Input: nums = [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
**Example 2:**
```
Input: nums = [2,3,0,1,4]
Output: 2
``` 

**Constraints:**

- $1 <= nums.length <= 10^4$
- `0 <= nums[i] <= 1000`
- It's guaranteed that you can reach `nums[n - 1]`.

## Solution

採用 DP

```kotlin
class Solution {
    fun jump(nums: IntArray): Int {
        val n = nums.size
        if (n == 1) return 0

        // dp[i]: 從 i 格出發，到最後一格的最小跳躍數
        val dp = IntArray(n) { Int.MAX_VALUE }

        // 最後一格不用跳
        dp[n - 1] = 0

        // 起點從倒數第二格開始，往前遞減
        for (start in n - 2 downTo 0) {

            // 找到由 start 出發，到最後一格的最小跳躍數
            var minJumps = Int.MAX_VALUE
            for (step in 1..nums[start]) {
                if (start + step < n && dp[start + step] != Int.MAX_VALUE) {
                    minJumps = min(minJumps, dp[start + step])
                }
            }
            dp[start] = if (minJumps != Int.MAX_VALUE) 1 + minJumps else minJumps
        }

        // 返回 dp[0]，即由第一格出發，到最後一格的最小跳躍數
        return dp[0]
    }
}
```

Greedy

```kotlin
class Solution {
    fun jump(nums: IntArray): Int {
        val n = nums.size
        if (n == 1) return 0

        var cur = 0
        var last = 0
        var jumps = 0

        for (i in 0 until n) {
            // 更新目前起點最遠可達距離
            cur = max(cur, i + nums[i])

            // 如果上一次最遠可達距離等於目前起點
            if (i == last) {
                // 更新上一次最遠可達距離
                last = cur 
                
                // 增加跳躍數
                jump++

                // 如果目前最遠可達距離 大於或等於 最後一格   
                if (cur >= n - 1) break
            }
        }

        return jumps
    }
}
```