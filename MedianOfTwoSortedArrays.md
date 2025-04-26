# 4. Median of Two Sorted Arrays

Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.


**Example 1:**
```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```
**Example 2:**
```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
``` 

**Constraints:**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- $-10^6 <= nums1[i], nums2[i] <= 1^6$


## Solution

```kotlin
class Solution {
    fun findMedianSortedArrays(nums1: IntArray, nums2: IntArray): Double {
        val size1 = nums1.size
        val size2 = nums2.size
        val half = ((size1 + size2) / 2)

        // 選擇較短陣列為 a
        val a = if (size1 < size2) nums1 else nums2
        val b = if (size1 < size2) nums2 else nums1

        var left = 0
        var right = a.size - 1

        while (true) {
            // 在 a 做 binary search，
            
            // i 指向的是 a 中會出現的 左半邊的最大數字，a[0:i] 會出現在左半邊
            // 需考慮 right 為 -1 情況，所以使用 Math.floorDiv，確保 i = (0 + -1)/2 = -1
            val i = Math.floorDiv(left + right, 2)
            
            // j 指向的是 b 中會出現的 左半邊的最大數字，b[0:j] 會出現在左半邊
            // j = (half - (i+1)) - 1 = half - i - 2
            val j = half - i - 2

            val aLeft = if (i >= 0) a[i] else Int.MIN_VALUE
            val aRight = if (i + 1 < a.size) a[i + 1] else Int.MAX_VALUE

            val bLeft = if (j >= 0) b[j] else Int.MIN_VALUE
            val bRight = if (j + 1 < b.size) b[j + 1] else Int.MAX_VALUE

            if (aLeft <= bRight && bLeft <= aRight) {
                if ((size1 + size2) % 2 == 1) {
                    return min(aRight, bRight).toDouble()
                } else {
                    return (max(aLeft, bLeft).toDouble() + min(aRight, bRight).toDouble())/2
                }
            } else if (aLeft > bRight){
                // a too long
                right = i - 1
            } else {
                left = i + 1
            }
        }
    }
}
```