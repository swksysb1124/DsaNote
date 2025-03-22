## 416. Partition Equal Subset Sum

Given an integer array `nums`, return `true` *if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or `false` otherwise*.

**Example 1:**
```
Input: nums = [1,5,11,5]
Output: true
Explanation: The array can be partitioned as [1, 5, 5] and [11].
```
**Example 2:**
```
Input: nums = [1,2,3,5]
Output: false
Explanation: The array cannot be partitioned into equal sum subsets.
``` 

**Constraints:**

- `1 <= nums.length <= 200`
- `1 <= nums[i] <= 100`

## Solution

這是一個 **0-1 背包問題**。

**2D 動態規劃**
```kotlin
class Solution {
    fun canPartition(nums: IntArray): Boolean {
        val sum = nums.sum() 
        if (sum % 2 != 0) return false
        
        val n = nums.size
        val target = sum / 2

        // dp[i][j]: 當包含到第 i nums, 目標為 j, 是否有解
        val dp = Array(n + 1) {
            BooleanArray(target + 1) { false }
        }

        // 當 target 為零，可以有解
        for (i in 0..n) { dp[i][0] = true }

        for (i in 1..n) {
            for (j in 1..target) {
                if (j >= nums[i - 1]) {
                    // 有放 i num 或 沒放 i num
                    dp[i][j] = dp[i - 1][j - nums[i - 1]] || dp[i - 1][j]
                } else {
                    dp[i][j] = dp[i - 1][j]
                }
            }
        }

        return dp[n][target]
    }
}
```

**1D 動態規劃**

```kotlin
class Solution {
    fun canPartition(nums: IntArray): Boolean {
        val sum = nums.sum() 
        if (sum % 2 != 0) return false
        
        val n = nums.size
        val target = sum / 2

        // dp[j]: 目標為 j, 是否有解
        val dp = BooleanArray(target + 1) { false }
        

        // 當 target 為零，可以有解
        dp[0] = true 

        for (i in 0 until n) {
            for (j in target downTo 0) { // 必須反向！！！
                if (j >= nums[i]) {
                    // 有放 i num 或 沒放 i num
                    dp[j] = dp[j - nums[i]] || dp[j]
                }
            }
        }

        return dp[target]
    }
}
```