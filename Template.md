# 常用演算法模板

## DFS 
```kotlin
class Node(val value: Int) {
    val children = mutableListOf<Node>()
}

fun dfs(root: Node?) {
    if (root == null) return 

    // 判定 root 是否有效、是否加上 visited ...

    // 可以先處理當前 node
    
    // 處理 children
    for (node in children) {
        dfs(node)
    }
}
```

## BFS

```kotlin
class Node(val value: Int) {
    val children = mutableListOf<Node>()
}

fun bfs(start: Node?) {
    if (start == null) return

    // 可能需要紀錄節點是否重複
    val visited = HashSet<Node>()

    val queue = LinkedList<Node>()
    queue.offer(start)

    while (queue.isNotEmpty()) {
        var size = queue.size
        while (size-- > 0) {
            // 處理當前節點
            val curr = queue.poll()
            // 如果當前節點符合條件，表示成功
            for (node in curr.children) {
                if (isValid(node) && !visited.contains(node)) {
                    queue.offer(node)
                    visited.add(node)
                }
            }
        }
    }
}
```     