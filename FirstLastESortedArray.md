# 34. Find First and Last Position of Element in Sorted Array

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

 
**Example 1:**
```
Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]
```

**Example 2:**
```
Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]
```

**Example 3:**
```
Input: nums = [], target = 0
Output: [-1,-1]
```

**Constraints:**
- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- `nums` is a non-decreasing array.
- `-10^9 <= target <= 10^9`

## Solution
### Binary Search

class Solution {
    fun searchRange(nums: IntArray, target: Int): IntArray {
        // corner case
        if (nums.isEmpty()) return intArrayOf(-1, -1)

        val left = leftBound(nums, target)
        val right = rightBound(nums, target)
        return intArrayOf(left, right)
    }

    private fun leftBound(nums: IntArray, target: Int): Int {
        val n = nums.size
        var left = 0
        var right = n - 1
        while (left <= right) {
            val mid = left + (right - left)/2
            if (nums[mid] == target) {
                right = mid - 1
            } else if (nums[mid] < target) {
                left = mid + 1
            } else if (nums[mid] > target) {
                right = mid - 1
            }
        }

        return if (left < n && left >= 0 && nums[left] == target) left else -1
    }

    private fun rightBound(nums: IntArray, target: Int): Int {
        val n = nums.size
        var left = 0
        var right = n - 1
        while(left <= right) {
            val mid = left + (right - left)/2
            if (nums[mid] == target) {
                left = mid + 1
            } else if (nums[mid] < target) {
                left = mid + 1
            } else if (nums[mid] > target) {
                right = mid - 1
            }
        }

        return if (right < n && right >= 0 && nums[right] == target) right else -1
    }
}