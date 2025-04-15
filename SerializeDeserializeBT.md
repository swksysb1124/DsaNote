# 297. Serialize and Deserialize Binary Tree

## Solution
```kotlin
/**
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */

class Codec() {
	// Encodes a URL to a shortened URL.
    fun serialize(root: TreeNode?): String {
        // 處理只有 null 的例子
        if (root == null) return "null"
        
        val sb = StringBuilder()
        
        // pre-order traversal 遞迴
        fun seHelper(root: TreeNode?) {
            // 如果為 null, 加入 null
            if (root == null) {
                sb.append("null").append(",")
                return 
            }
            // 加入 root
            sb.append(root.`val`).append(",")
            seHelper(root.left)
            seHelper(root.right)
        }

        // 開始遞迴
        seHelper(root)
        val result = sb.toString()
        val n = result.length

        // 如果最後有帶','，刪除掉它 
        return if (result[n - 1] == ',') {
            result.substring(0, n - 1)
        } else {
            result
        }
    }

    // Decodes your encoded data to tree.
    fun deserialize(data: String): TreeNode? {
        // 考慮只有一個 null 的情況
        if (data == "null") return null

        // 轉化成 nodeList
        val nodeList = data.split(",").toList().toMutableList()

        fun deHelper(nodeList: MutableList<String>): TreeNode? {
            // 沒有 node，回傳 null
            if (nodeList.isEmpty()) return null
            
            // 因為是 pre-order，所以第一個是 root
            val firstValue = nodeList.removeFirst()

            // 考慮 root 為 null 情況
            if (firstValue == "null") return null
            
            // 生成 root
            val root = TreeNode(firstValue.toInt())
            root.left = deHelper(nodeList)
            root.right = deHelper(nodeList)
            return root
        }

        return deHelper(nodeList)
    }
}
```