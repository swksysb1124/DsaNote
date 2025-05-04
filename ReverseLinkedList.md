# 206. Reverse Linked List

Given the head of a singly linked list, reverse the list, and return the reversed list.

 

Example 1:


Input: head = [1,2,3,4,5]
Output: [5,4,3,2,1]
Example 2:


Input: head = [1,2]
Output: [2,1]
Example 3:

Input: head = []
Output: []
 

Constraints:

The number of nodes in the list is the range [0, 5000].
-5000 <= Node.val <= 5000
 

Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?

## Solution
```kotlin
class Solution {
    fun reverseList(head: ListNode?): ListNode? {
        // 只剩一個 node 直接回傳為新的 head
        if (head == null || head.next == null) return head

        var node = head
        // 將 node.next 之後做遞回返為新的 head
        // 這個遞回會一直傳到最後一個 node
        val newHead = reverseList(node.next)
        // 反轉 node 跟 node.next 的指向
        node?.next?.next = node
        node?.next = null
        return newHead
    }
}
```