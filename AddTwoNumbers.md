# 2. Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

**Example 2:**
```
Input: l1 = [0], l2 = [0]
Output: [0]
```

**Example 3:**
```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
``` 

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is **guaranteed** that the list represents a number that does not have leading zeros.


## Solution

核心邏輯：首先產生一個 `dummy` 節點 方便我們最後取得 new list node 的 head，然後由一個 `curr` 指向 `dummy`。 

因為是倒序，可以用兩個指針指向 `l1`、`l2`，加總 l1 跟 l2 數值 跟 carry，計算結果生成新的 node 加在 curr 後面
每處理完一個 `l1` 跟 `l2` 的節點， `curr` 、 `l1` 跟 `l2` 再往右移一格，便於計算下一個 digit。

如果 `l1` 或 `l2` 其中一個為 `null` 或是 `carry` 還有剩，要計算走完回圈，才不會漏掉進位跟比較高的位數。

``kotlin
class Solution {
    fun addTwoNumbers(l1: ListNode?, l2: ListNode?): ListNode? {
        if (l1 == null && l2 == null) return null
        
        var node1: ListNode? = l1
        var node2: ListNode? = l2
        val dummy = ListNode(0)
        var cur: ListNode? = dummy
        var carry = 0

        while (node1 != null || node2 != null || carry != 0) {
            val x = if (node1 != null) node1.`val` else 0
            val y = if (node2 != null) node2.`val` else 0
            val sum = x + y + carry
            carry = sum / 10
            val newNode = ListNode(sum % 10)
            cur?.next = newNode
            cur = cur?.next
            node1 = node1?.next
            node2 = node2?.next
        }

        return dummy.next
    }
}
```