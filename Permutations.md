# 46. Permutations

Given an array `nums` of distinct integers, return all the possible 
permutations. You can return the answer in **any order**.

**Example 1:**
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```
**Example 2:**
```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```
**Example 3:**
```
Input: nums = [1]
Output: [[1]]
```

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- `All the integers of nums are unique.`

## Solution
### backtracking
```kotlin
class Solution {
    fun permute(nums: IntArray): List<List<Int>> {
        val track = mutableList<Int>()
        val res = MutableList<List<Int>>()

        fun backtracking(nums: IntArray) {
            if (track.size == nums.size) {
                res.add(track.toList())
                return
            }

            for (num in nums) {
                // 如果 track 已包含選擇，跳過
                if (track.contains(num)) continue

                // 加入選擇
                track.add(num)

                backtracking(num)

                // 移除選擇
                track.remove(num)
            }
        }

        backtracking(nums)
    
        return res
    }
}
```
