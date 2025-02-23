# Binary Search

```kotlin
fun binarySearch(nums: IntArray, target: Int): Int {
    val n = nums.size
    var left = 0
    var right = n - 1
    while (left <= right) {
        val mid = left + (right - left)/2

        if (nums[mid] == target) {
            return mid
        } else if (nums[mid] < target) {
            left = mid + 1
        } else if (nums[mid] > target) {
            right = mid - 1
        }
    }

    return -1
}
```