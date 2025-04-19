# 359. Logger Rate Limiter 

Design a logger system that receives a stream of messages along with their timestamps. Each **unique** message should only be printed **at most every 10 seconds** (i.e. a message printed at timestamp `t` will prevent other identical messages from being printed until timestamp `t + 10`).

All messages will come in chronological order. Several messages may arrive at the same timestamp.

Implement the `Logger` class:

- `Logger()` Initializes the logger object.
- `bool shouldPrintMessage(int timestamp, string message)` Returns `true` if the `message` should be printed in the given `timestamp`, otherwise returns `false`.

**Example 1:**
```
Input
["Logger", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage", "shouldPrintMessage"]
[[], [1, "foo"], [2, "bar"], [3, "foo"], [8, "bar"], [10, "foo"], [11, "foo"]]

Output
[null, true, true, false, false, false, true]

Explanation
Logger logger = new Logger();
logger.shouldPrintMessage(1, "foo"); // return true, next allowed timestamp for "foo" is 1 + 10 = 11
logger.shouldPrintMessage(2, "bar"); // return true, next allowed timestamp for "bar" is 2 + 10 = 12
logger.shouldPrintMessage(3, "foo"); // 3 < 11, return false
logger.shouldPrintMessage(8, "bar"); // 8 < 12, return false
logger.shouldPrintMessage(10, "foo"); // 10 < 11, return false
logger.shouldPrintMessage(11, "foo"); // 11 >= 11, return true, next allowed timestamp for "foo" is 11 + 10 = 21
 ``` 
 

**Constraints:**

- $0 <= timestamp <= 10^9$
- Every `timestamp` will be passed in non-decreasing order (chronological order).
- `1 <= message.length <= 30`
- At most $10^4$ calls will be made to `shouldPrintMessage`.

## Solution
使用一個 `HashMap` 來紀錄**訊息**跟**時間戳**。當訊息存在紀錄，且**新時間戳**與**既有時間戳**差距**小於 10**，判定為 `false`，其他情況視為 `true`，並記錄跟訊息的時間戳。

```kotlin
class Logger {
    private val messageTimestamp = HashMap<String, Int>()

    fun shouldPrintMessage(timestamp: Int, message: String): Boolean {
        val existedTimestamp = messageTimestamp[message]
        if (existedTimestamp != null && timestamp - existedTimestamp < 10) return false
        messageTimestamp[message] = timestamp
        return true
    }
}
```