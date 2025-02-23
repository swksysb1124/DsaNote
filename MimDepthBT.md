# 111. Minimum Depth of Binary Tree

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

**Example 1:**

```
Input: root = [3,9,20,null,null,15,7]
Output: 2
```

**Example 2:**
```
Input: root = [2,null,3,null,4,null,5,null,6]
Output: 5
``` 

**Constraints:**

- The number of nodes in the tree is in the range `[0, 10^5]`.
- `-1000 <= Node.val <= 1000`


## Solution

### BFS
```kotlin
class Solution {
    fun minDepth(root: TreeNode?): Int {
        if (root == null) return 0

        val queue = LinkedList<TreeNode>()
        queue.offer(root)
        var depth = 1

        while(queue.isNotEmpty()) {
            // 取同一層個數
            var levelNum = queue.size

            // 取出同一層元素
            for (i in 0 until levelNum) {
                // 判定是否符合條件
                val curr = queue.poll()
                if (curr.left == null && curr.right == null) {
                    return depth
                }

                // 加入 curr 相鄰節點(或子節點)
                if (curr.left != null) queue.offer(curr.left)
                if (curr.right != null) queue.offer(curr.right)
            }
            
            // 更新深度
            depth++
        }

        return depth
    }
}
```