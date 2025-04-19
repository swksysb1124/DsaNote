# 202. Happy Number

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:

- Starting with any positive integer, replace the number by the sum of the squares of its digits.
- Repeat the process until the number equals `1` (where it will stay), or it **loops endlessly in a cycle** which does not include `1`.
- Those numbers for which this process **ends in `1`** are happy.

Return `true` if `n` is a *happy number*, and `false` *if not*.

 

**Example 1:**
```
Input: n = 19
Output: true
Explanation:
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

**Example 2:**
```
Input: n = 2
Output: false
``` 

**Constraints:**

- $1 <= n <= 2^31 - 1$

## Solution

**Method 1**
```kotlin
class Solution {
    fun isHappy(n: Int): Boolean {
        // create a set to record used number
        val used = HashSet<Int>()

        // initialize num as n
        var num = n

        while (!used.contains(num)) {
            // mark the num as used
            used.add(num)

            // calculate digit square sum
            var sum = 0
             while (num != 0) {
                sum += (num % 10) * (num % 10)
                num /= 10
            }

            // if sum equals one, return true
            if (sum == 1) return true
            
            // update num
            num = sum
        }
        return false
    }
}
```

| Time Complexity | Space Complexity |
|:---:|:---:|
| $O(N)$ | $O(N)$ |

**Method 2: Fast and Slow pointer**
```kotlin
class Solution {
    fun isHappy(n: Int): Boolean {
        var slow = n
        var fast = getNextNumber(n)

        while (fast != slow && fast != 1) {
            slow = getNextNumber(slow)
            fast = getNextNumber(getNextNumber(fast))
        }

        return fast == 1
    }

    private fun getNextNumber(num: Int): Int {
        var sum = 0
        var n = num
        while (n != 0) {
            val digit = n % 10
            sum += digit * digit
            n /= 10
        }
        return sum
    }
}
```

| Time Complexity | Space Complexity |
|:---:|:---:|
| $O(N)$ | $O(1)$ |