# [Leetcode] 189. Rotate Array

**`Medium` `Array`**

## Description

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.


### Example 1:
```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]
Explanation:
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]
```

### Example 2:
```
Input: nums = [-1,-100,3,99], k = 2
Output: [3,99,-1,-100]
Explanation: 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]
```

### Constraints:
- `1 <= nums.length <= 105`
- `-2^31 <= nums[i] <= 2^31 - 1`
- `0 <= k <= 105`


## Solution

### Right shift 

*Kotlin*
```kotlin
class Solution {
    fun rotate(nums: IntArray, k: Int) {
        val n = nums.size
        if (n == 0) return
        // get k % n to avoid unnecessary rotation
        val order = k % n

        for (i in 1..order) {
            val last = nums[n-1]
            for (j in n-1..1) {
                nums[j] = nums[j-1]
            }
            nums[0] = last
        }
    }	
}
```
*Python*
```python
class Solution(object):
  def rotate(self, nums, k):
        n = len(nums)
        if n == 0: 
            return
        order = k % n

        for i in range(order):
            last = nums[n-1]
            for j in range(n-1, 0, -1):
                nums[j] = nums[j-1]
            nums[0] = last
```

### Bobble shift
*Kotlin*
```kotlin
class Solution {
    fun rotate(nums: IntArray, k: Int) {
        val n = nums.size
        if (n == 0) return
        val order = k % n

        for (i in 1..order) {
            for (j in n-1..1) {
                val temp = nums[j]
                nums[j] = nums[j-1]
                nums[j-1] = temp 
            }
        }
    }
}
```
*Python*
```python
class Solution(object):
    def rotate(self, nums, k):
        n = len(nums)
        if n == 0:
            return
        order = k % n
        
        for i in range(order):
            for j in range(n-1, 0, -1):
                temp = nums[j]
                nums[j] = nums[j-1]
                nums[j-1] = temp
```

### Reverse
*Kotlin*
```kotlin
class Solution {
    fun rolate(nums: IntArray, k: Int) {
        val n = nums.size
        if (n == 0) return
        val order = k % n

        reverse(0, n-order-1, nums)
        reverse(n-order, n-1, nums)
        reverse(0, n-1, nums)
    }

    private fun reverse(start: Int, end: Int, nums: IntArray) {
        var l = start
        var r = end
        while (l < r) {
            val temp = nums[l]
            nums[l] = nums[r]
            nums[r] = temp
            l++
            r--
        }
    }
}
```

*Python*
```python
class Solution(object):
    def rotate(self, nums, k):
        n = len(nums)
        if n == 0:
            return
        order = k % n

        self.reverse(0, n-order-1, nums)
        self.reverse(n-order, n-1, nums)
        self.reverse(0, n-1, nums)


    def reverse(self, start, end, nums):
        l, r = start, end
        while l < r:
            nums[l], nums[r] = nums[r], nums[l]
            l+=1
            r-=1
```
