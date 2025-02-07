# Linked List 結構

**資料結構**

*Kotlin*
```kotlin
class LinkedNode(val v: Int) {
    var next: LinkedNode? = null
}

// 使用 while loop 遍歷 list node
// 會依序印出 node
fun traverseByLoop(head: LinkedNode) {
    var curr: LinkedNode? = head
    while (curr != null) {
        print(curr.v + " ")
        curr = curr.next
    }
}

// 使用遞迴進行 pre order 遍歷
// 會依序印出 node
fun traversePreOrder(head: LinkedNode?) {
    if (head == null) return
    print(curr.v + " ")
    traversePreOrder(head.next)
}

// 使用遞迴進行 post order 遍歷
// 會倒序印出 node
fun traversePostOrder(head: LinkedNode?) {
    if (head == null) return
    traversePostOrder(head.next)
    print(curr.v + " ")
}

```
