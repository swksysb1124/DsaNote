# Binary Search

```kotlin
fun binarySearch(nums: IntArray, target: Int): Int {
    val n = nums.size
    var left = 0
    var right = n - 1
    // 閉區間（頭尾包含解）
    while (left <= right) {

        // 取中間值，避免溢位
        val mid = left + (right - left)/2
        
        // 明確枚舉 等於、大於、小於 情況
        if (nums[mid] == target) {
            // 中間值等於目標，取得解
            return mid
        } else if (nums[mid] < target) {
            // 中間值小於目標，目標在右半部，更新左邊界
            left = mid + 1
        } else if (nums[mid] > target) {
            // 中間值大於目標，目標在左半部，更新右邊界
            right = mid - 1
        }
    }
    // 都找不到，回傳無效值
    return -1
}
```