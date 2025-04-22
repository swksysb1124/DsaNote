#  560. Subarray Sum Equals K

Given an array of integers `nums` and an integer `k`, return *the total number of subarrays whose sum equals to* `k`.

A subarray is a contiguous **non-empty** sequence of elements within an array.

 

**Example 1:**
```
Input: nums = [1,1,1], k = 2
Output: 2
```
**Example 2:**
```
Input: nums = [1,2,3], k = 3
Output: 2
``` 

**Constraints:**

- $1 <= nums.length <= 2 * 10^4$
- $-1000 <= nums[i] <= 1000$
- $-10^7 <= k <= 10^7$

## Solution
**暴力解**

可以使用兩個迴圈來決定掃描的上下限，在上下限中計算總和，並在總和等於 `k`，加入計算。
此時間複雜度為 $O(N^3)$

```kotlin
class Solution {
    fun subarraySum(nums: IntArray, k: Int): Int {
        var count = 0
        val n = nums.size
        for (s in 0..<n){
            for (e in s + 1..n) {
                // get sum in the [s, e) interval
                var sum = 0
                for (i in s..<e) {
                    sum += nums[i]
                }
                if (sum == k) count++
            }
        }
        return count
    }
}
```

**Prefix Sum + HashMap**

核心概念：
- `PrefixSum(i) == k`，表示 `nums[0]` ~ `nums[i]` 的總和等於 `k`。

- 如果 `PrefixSum(i) - k = PrefixSum(j)`，表示 `nums[j+1] ~ nums[i]` 的總和等於 `k`。

作法：

- 使用一個 `HashMap` 紀錄 `PrefixSum` 跟 **出現次數**。

- 使用一個迴圈遍歷 `nums` 每個數，同時計算 `prefixSum(i)` 
  
- 如果發現 `prefixSum(i)` 等於 `k`，加總一次
  
- 如果發現 `HashMap` 中，有包含 `prefixSum(i) - k` 的數字，表示之前存在 `prefixSum(j)`，使得 `prefixSum(i) - k ＝ prefixSum(j)`，因此把 `prefixSum(j)` 次數都算進來。
  
- 最後更新 `prefixSum(i)` 次數


時間複雜度：$O(N)$，空間複雜度：$O(N)$

```kotlin
class Solution {
    fun subarraySum(nums: IntArray, k: Int): Int {
        val map = HashMap<Int, Int>()
        var res = 0
        var sum = 0
        for (i in 0..<nums.size) {
            sum += nums[i] // 計算前綴和
            if (sum == k) {
                res++ // 若目前前綴和剛好是 k，表示 [0..i] 是一個合法子陣列
            }
            
            if (map.containsKey(sum - k)) {
                res += map[sum - k]!! // 有幾個 sum_j 讓 sum_i - sum_j == k，就加幾次
            }
            
            map[sum] = (map[sum] ?: 0) + 1 // 更新當前前綴和次數
        }
        return res
    }
}
```