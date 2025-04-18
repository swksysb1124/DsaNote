# 366. Find Leaves of Binary Tree

## Solution
**DFS**
```kotlin
class Solution {
    fun findLeaves(root: TreeNode): List<List<Int>> {
        val res = mutableListOf(mutableListOf<Int>())
        dfs(root, res)
        return res
    }

    private fun dfs(root: TreeNode?, res: MutableList<MutableList<Int>>): Int {
        if (root == null) return 0

        val leftLeft = dfs(root.left, res)
        val rightLeft = dfs(root.right, res)
        // 取左右最大為該節點 leve
        val level = max(leftLeft, rightLeft)
        // level 等於 res.size，表示該 level 為空
        if (res.size == level) {
            res.add(mutableListOf())
        }
        res[level].add(root.value)
        return level + 1
    }
}

class TreeNode(
    var value: Int,
    var left: TreeNode? = null,
    var right: TreeNode? = null
)
```


Time Complexity: 
$
O(N)
$