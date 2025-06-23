# 274. H-Index

Given an array of integers `citations` where `citations[i]` is the number of citations a researcher received for their $i^{th}$ paper, return *the researcher's h-index*.

According to the definition of h-index on Wikipedia: The h-index is defined as the maximum value of `h` such that the given researcher has published at least `h` papers that have each been cited at least `h` times.

 

**Example 1:**
```
Input: citations = [3,0,6,1,5]
Output: 3
Explanation: [3,0,6,1,5] means the researcher has 5 papers in total and each of them had received 3, 0, 6, 1, 5 citations respectively.
Since the researcher has 3 papers with at least 3 citations each and the remaining two with no more than 3 citations each, their h-index is 3.
```

**Example 2:**
```
Input: citations = [1,3,1]
Output: 1
``` 

**Constraints:**
- `n == citations.length`
- `1 <= n <= 5000`
- `0 <= citations[i] <= 1000`

## Solution
暴力解
```kotlin
class Solution {
    fun hIndex(citations: IntArray): Int {
        val n = citations.size
        for (i in n downTo 1) {
            var num = 0
            for (c in citations) {
                if (c >= i) {
                    num++
                }
            }
            if (num >= i) return i
        }
        return 0
    }
}
```
時間 / 空間複雜度： $O(N*M)$ / $O(1)$

Greedy + Sorting

```kotlin
class Solution {
    fun hIndex(citations: IntArray): Int {
        val n = citations.size
        val cits = citations.sortDescending()
        var x = 0
        while (x < n && citations[x] >= x + 1) {
            x++
        }
        return x
    }
}
```

時間 / 空間複雜度： $O(N * logN)$ / $O(1)$ (視 sorting 實作)

桶排序法

```kotlin
class Solution {
    fun hIndex(citations: IntArray): Int {
        val n = citations.size
        // 引用數 v.s. 論文數
        val buckets = IntArray(n + 1) { 0 }

        // 如果引用數大於或等於論文總數，放到 引用數為 n 桶
        // 如果引用數小於論文總數，放到對應引用數的桶子
        for (c in citations) {
            if (c >= n) buckets[n]++ 
            else buckets[c]++
        }

        var total = 0
        // 由引用數 n 的桶子開始用引用數小的桶子加總(論文數)
        for (i in n downTo 0) {
            total += buckets[i]

            // 如果目前加總論文數目大於或等於引用數
            if (total >= i) {
                // 此引用數即為 h-index
                return i
            }
        }

        return 0
    }
}
```

時間 / 空間複雜度： $O(N)$ / $O(Ｎ)$

