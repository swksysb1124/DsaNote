# 450. Delete Node in a BST

## Solution
```kotlin
/**
 * Example:
 * var ti = TreeNode(5)
 * var v = ti.`val`
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */
class Solution {
    fun deleteNode(root: TreeNode?, key: Int): TreeNode? {
        if (root == null) return null
        if (root.`val` == key) {
            // delete node according to different cases
            // case 1 & 2: no child and one child
            if (root.left == null) return root.right
            if (root.right == null) return root.left
            // case 3: two children
            // find min in right substree.
            // copy min to current root
            // delete the min from right substree.
            val min = getMin(root.right)?.`val` ?: 0
            root.`val` = min
            root.right = deleteNode(root.right, min)
        } else if (root.`val` > key) {
            root.left = deleteNode(root.left, key)
        } else {
            root.right = deleteNode(root.right, key)
        }
        return root
    }

    fun getMin(root: TreeNode?): TreeNode? {
        var node = root
        while (node?.left != null) {
            node = node?.left
        }
        return node
    }
}
```