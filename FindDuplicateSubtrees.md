# 652. Find Duplicate Subtrees

Given the `root` of a binary tree, return all   .

For each kind of duplicate subtrees, you only need to return the root node of any **one** of them.

Two trees are **duplicate** if they have the **same structure** with the **same node values**.

 

**Example 1:**
```
Input: root = [1,2,3,4,null,2,4,null,null,4]
Output: [[2,4],[4]]
```

**Example 2:**
```
Input: root = [2,1,1]
Output: [[1]]
```

**Example 3:**
```
Input: root = [2,2,2,3,null,3,null]
Output: [[2,3],[3]]
``` 

**Constraints:**

 - The number of the nodes in the tree will be in the range `[1, 5000]`
- `-200 <= Node.val <= 200`

## Solution

核心觀念：

使用 DFS 來 serialize subtree 為 字串，再利用 HashMap 來記錄 subtree 字串的出現一次以上，就將其列入最後輸出 list 中。

```kotlin
class Solution {
    fun findDuplicateSubtrees(root: TreeNode?): List<TreeNode?> {
        val res = mutableListOf<TreeNode?>()
        val substrees = HashMap<String, MutableList<TreeNode>>()

        fun dfs(root: TreeNode?): String {
            if (root == null) return "null"
            val s = "${root.`val`}" + "," + dfs(root.left) + "," + dfs(root.right)
            
            if ((substrees[s]?.size ?: 0) == 1) {
                res.add(root)
            }
            substrees.putIfAbsent(s, mutableListOf<TreeNode>())
            substrees[s]?.add(root)
            return s
        }

        dfs(root)
        return res
    }
}
```