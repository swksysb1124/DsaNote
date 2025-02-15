# 費波那契數列

## 轉移方程式


## 演算法
### 暴力解法
```kotlin
fun fib(n: Int): Long {
    // base case
    if (n == 0) return 0
    if (n == 1 || n == 2) return 1

    return fib(n - 1) + fib(n - 2) 
}
```
時間複雜度：`O(2^n)`


### 備忘錄解法
```kotlin
fun fib(n: Int): Long {
    val memo = LongArray(n + 1) { -1 }

    // Top-down
    fun helper(n: Int): Long {
        // 如果備忘錄已有紀錄，返回紀錄
        if (memo != -1) return memo[n]

        // base case
        if (n == 0) return 0
        if (n == 1 || n == 2) return 1

        // 更新備忘錄
        memo[n] = helper(n - 1) + helper(n - 2) 
        return memo[n]
    }

    return helper[n]
}
```
時間複雜度：`O(n)`

## 動態規劃解法
```kotlin
fun fib(n: Int): Long {
    val dp = LongArray(n + 1){ 0 }

    // base case
    dp[1] = 1
    dp[2] = 1

    // Bottom-up
    for (i in 3..n) {
      dp[i] = dp[i - 1] + dp[i - 2]
    }
    return dp[n]
}
```
時間複雜度：`O(n)`
