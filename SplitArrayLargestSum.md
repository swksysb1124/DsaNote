# 410. Split Array Largest Sum

Given an integer array `nums` and an integer `k`, split nums into `k` non-empty subarrays such that the largest sum of any subarray is **minimized**.

Return *the **minimized** largest sum of the split*.

A **subarray** is a contiguous part of the array.

 

**Example 1:**
```
Input: nums = [7,2,5,10,8], k = 2
Output: 18
Explanation: There are four ways to split nums into two subarrays.
The best way is to split it into [7,2,5] and [10,8], where the largest sum among the two subarrays is only 18.
```
**Example 2:**
```
Input: nums = [1,2,3,4,5], k = 2
Output: 9
Explanation: There are four ways to split nums into two subarrays.
The best way is to split it into [1,2,3] and [4,5], where the largest sum among the two subarrays is only 9.
``` 

**Constraints:**

- <code>1 <= nums.length <= 1000</code>
  
- <code>0 <= nums[i] <= 10<sup>6</sup></code>

- <code>1 <= k <= min(50, nums.length)</code>


## Solution

**DP**
 
這是 **動態規劃 + 前綴和** 的組合問題：

- 狀態定義：
  - `dp[i][j]` 表示：將前 `j` 個元素分成 `i` 段，其 **「最大子陣列和的最小值」**。
  
- 狀態轉移：
    - 對於每個可能的切割點 `k`（`i - 1 ≤ k < j`）：
  - 把前 `k` 個分成 `i-1` 段（`dp[i-1][k]`）
  - 把第 `i` 段設為 `nums[k] ~ nums[j-1]`
  - 此時這種切法的最大值是 `max(dp[i-1][k], sum[k..j-1])`
  - 在所有可能的 `k` 中選最小的這個最大值
  
- 前綴和：
  - 使用 `sums[]` 儲存前綴和，讓你可以快速取得任一段的和 `sum[k..j-1] = sums[j] - sums[k]`

```
dp[i][j] =min(
    max(dp[i - 1][i - 1], prefix[j] - prefix[i - 1]),
    max(dp[i - 1][i], prefix[j] - prefix[i]),
    ..., 
    max(dp[i - 1][j - 1], prefix[j] - prefix[j - 1])
)
```
```kotlin
class Solution {
    fun splitArray(nums: IntArray, k: Int): Int {
        val n = nums.size

        val preSums = IntArray(n + 1) { 0 }
        for (i in 1..n) {
            preSums[i] = preSums[i - 1] + nums[i - 1]
        }

        // dp[i][j]: 前j個元素分成i組
        val dp = Array(k + 1) { IntArray(n + 1) { Int.MAX_VALUE }}
        dp[0][0] = 0

        for (i in 1..k) { // i 幾組
            for (j in 1..n) { // j 元素個數
                for (x in (i - 1)..<j) {
                    val max = max(dp[i - 1][x], preSums[j] - preSums[x])
                    dp[i][j] = min( dp[i][j], max)
                }
            }
        }

        return dp[k][n]
    }
}
```

**Binary Search**

 **核心想法（Binary Search + Greedy）：**

- 猜一個最大值 `maxSum`（也就是某段總和的上限）
- 檢查是否能用這個 `maxSum` 劃成 `k` 段或更少
  - 這用貪心的方式：一旦超過 `maxSum`，就開始新的一段
- 如果可以 → 嘗試更小的 `maxSum`（縮小範圍）
- 如果不行 → 嘗試更大的 `maxSum`（放寬限制）

**關鍵：**
- 最小可能的 `maxSum` 是陣列中最大的數
- 最大可能的 `maxSum` 是整個陣列的總和
- 我們用 **二分搜尋法** 在這個範圍內找最小的合法 `maxSum`

```kotlin
class Solution {
    fun splitArray(nums: IntArray, k: Int): Int {
        fun canSplit(max: Int): Boolean {
            var subarray = 0
            var currSum = 0

            for (n in nums) {
                currSum += n 
                if (currSum > max) {
                    subarray += 1
                    currSum = n
                }
            }
            return subarray + 1 <= k
        }

        var l = nums.max()
        var r = nums.sum()

        var res = r
        while (l <= r) {
            val mid = (l + r)/2
            if (canSplit(mid)) {
                r = mid - 1
                res = mid
            } else {
                l = mid + 1
            }
        }

        return res
    }
}
```