# 198. House Robber

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

 

**Example 1:**
```
Input: nums = [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
```
**Example 2:**
```
Input: nums = [2,7,9,3,1]
Output: 12
Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
```

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

## Solution

**1D 動態規劃**
```kotlin
class Solution {
    fun rob(nums: IntArray): Int {
        val n = nums.size
        // dp[i] : 從 i 位置出發，能搶到的最多錢
        val dp = IntArray(n + 2) { 0 }

        for (i in n - 1 downTo 0) {
            dp[i] = max(
                dp[i + 1], // 不搶 i house
                dp[i + 2] + nums[i] // 搶 i house
            ) 
        }

        return dp[0]
    }
}
```

**常數動態規劃**

```kotlin
class Solution {
    fun rob(nums: IntArray): Int {
        val n = nums.size
        
        var res = 0
        var dp_1 = 0
        var dp_2 = 0

        for (i in n - 1 downTo 0) {
            res = max(
                dp_1, // 不搶 i house
                dp_2 + nums[i] // 搶 i house
            )
            dp_2 = dp_1
            dp_1 = res
        }

        return res
    }
}
```