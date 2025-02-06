# Tree 資料結構

## 二元樹 (Binary Tree)
*Kotlin*
```kotlin
class BinaryTreeNode(val v: Int) {
    var left: BinaryTreeNode? = null
    var right: BinaryTreeNode? = null
}
```

### 遍歷

*Kotlin*
```kotlin
fun traverse(node: BinaryTreeNode?) {
    if (node == null) return
    // preOrder
    traverse(node.left)
    // middleOrder
    traverse(node.right)
    // postOrder
}
```

## N元樹 (N Tree)
*Kotlin*
```kotlin
class TreeNode(val v: Int) {
    val children: MutableList<TreeNode> = mutableListOf<TreeNode>()
}
```

### 遍歷
```kotlin
fun traverse(node: TreeNode?) {
    // pre order
    for (child in node.children) {
        traverse(child)
    }
    // post order
}
```
