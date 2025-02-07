# Tree 資料結構

## 二元樹 (Binary Tree)
*Kotlin*
```kotlin
class BinaryTreeNode(val v: Int) {
    var left: BinaryTreeNode? = null
    var right: BinaryTreeNode? = null
}
```
 
*Python3*
```pythin3
class BinaryTreeNode:
    def __init__(self, v: int):
        self.v = v
        self.left = None
        self.right = None
```

### 遍歷

*Kotlin*
```kotlin
fun traverse(node: BinaryTreeNode?) {
    if (node == null) return
    // pre-order, print(node.v) or other handlings
    traverse(node.left)
    // in-order, print(node.v) or other handlings
    traverse(node.right)
    // post-order, print(node.v) or other handlings 
}
```

*Python3*
```python3
def traverse(node: Birthday):
    if node == None:
        return

    # pre-order traversal, print(node.v) or other handlings 
    traverse(node.left)
    # in-order traversal, print(node.v) or other handlings 
    traverse(node.right)
    # post-order travrsal, print(node.v) or other handlings is the 
```

## N元樹 (N Tree)
*Kotlin*
```kotlin
class TreeNode(val v: Int) {
    val children: MutableList<TreeNode> = mutableListOf<TreeNode>()
}
```

*Python3*
```python3
class TreeNode:
    def __init__(self, v: int):
        self.v = v
        self.children = []
```

### 遍歷
*Kotlin*
```kotlin
fun traverse(node: TreeNode?) {
    // pre-order traversal 
    for (child in node.children) {
        traverse(child)
    }
    // post-order travrsal 
}
```

*Python3*
```python3
def traverse(node: TreeNode):
    # pre-order 
    for child in node.children:
        traverse(child)
    # post-order 
```
