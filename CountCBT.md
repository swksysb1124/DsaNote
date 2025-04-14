# 222. Count Complete Tree Nodes

Given the `root` of a complete binary tree, return the number of the nodes in the tree.

According to **Wikipedia**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2^h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

 

**Example 1:**
```
Input: root = [1,2,3,4,5,6]
Output: 6
```

**Example 2:**
```
Input: root = []
Output: 0
```

**Example 3:**
```
Input: root = [1]
Output: 1
```
 

**Constraints:**
- The number of nodes in the tree is in the range `[0, 5 * 10^4]`.
- `0 <= Node.val <= 5 * 10^4`
- The tree is guaranteed to be **complete**.

## Solution

### First
```kotlin
class Solution {
    fun countNodes(root: TreeNode?): Int {
        if (root == null) return 0
        return 1 + countNodes(root.left) + countNodes(root.right)
    }
}
```
Time complexity: `O(N)`

### Better
```kotlin
class Solution {
    fun countNodes(root: TreeNode?): Int {
        if (root == null) return 0
        var left = root
        var hl = 0
        while (left != null) {
            left = left.left
            hl++
        }

        var right = root
        var hr = 0
        while (right != null) {
            right = right.right
            hr++
        }

        return if (hl == hr) {
            2f.pow(hl).toInt() - 1 // 滿二元樹
        } else {
            1 + countNodes(root.left) + countNodes(root.right) 
        }
    }
}
```

Time complexity: `O(logNlogN)`
