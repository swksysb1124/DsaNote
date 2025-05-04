# 24. Swap Nodes in Pairs

Given a linked list, swap every two adjacent nodes and return its head. You must solve the problem without modifying the values in the list's nodes (i.e., only nodes themselves may be changed.)

 

**Example 1:**
```
Input: head = [1,2,3,4]
Output: [2,1,4,3]
```

**Example 2:**
```
Input: head = []
Output: []
```

**Example 3:**
```
Input: head = [1]
Output: [1]
```

**Example 4:**
```
Input: head = [1,2,3]
Output: [2,1,3]
```
 

**Constraints:**

- The number of nodes in the list is in the range `[0, 100]`.
- `0 <= Node.val <= 100`


## Solution

由遞迴來處理，每次處理兩個節點的交換，再遞迴處理接下來的兩兩節點。

在遞迴函式中，
- **base case** 是當 linked list 只有一個或沒有節點了，這時候直接返回該節點的 head
- 設定第二個為 `newHead`，保留本來的兩兩節點之後的節點 (`tail`)
- 設定 `newHead` 下一個為本來的 `head`，head 下一個由遞回決定。遞回傳入 `tail`，也就是接下來 linked list 的起頭。

```kotlin
/**
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun swapPairs(head: ListNode?): ListNode? {
        // head 只有一個 node 或是沒有 直接回傳 head
        if (head == null || head.next == null) return head

        // 將第二個紀錄為新的 head 
        val newHead = head.next
        // 保存原本的 tail
        val tail = head.next.next
        // 將 newHead 下一個設定為 head
        newHead.next = head
        // head 下一個 node 需要由遞迴決定
        head.next = swapPairs(tail)
        // 返回新 head
        return newHead
    }
}
```