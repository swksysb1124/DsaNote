# 146. LRU Cache

Design a data structure that follows the constraints of a **Least Recently Used (LRU) cache**.

Implement the `LRUCache` class:

- `LRUCache(int capacity)` Initialize the LRU cache with positive size `capacity`.
- `int get(int key)` Return the value of the `key` if the `key` exists, otherwise return `-1`.
- `void put(int key, int value)` Update the value of the `key` if the `key` exists. Otherwise, add the `key-value` pair to the cache. If the number of keys exceeds the `capacity` from this operation, **evict** the least recently used key.

The functions `get` and `put` must each run in `O(1)` average time complexity.

 

**Example 1:**
```
Input
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
Output
[null, null, null, 1, null, -1, null, -1, 3, 4]

Explanation
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // cache is {1=1}
lRUCache.put(2, 2); // cache is {1=1, 2=2}
lRUCache.get(1);    // return 1
lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
lRUCache.get(2);    // returns -1 (not found)
lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
lRUCache.get(1);    // return -1 (not found)
lRUCache.get(3);    // return 3
lRUCache.get(4);    // return 4
``` 

**Constraints:**

- `1 <= capacity <= 3000`
- `0 <= key <= 10^4`
- `0 <= value <= 10^5`
- At most `2 * 10^5` calls will be made to `get` and `put`.


## Solution

**自定義資料結構**
```kotlin
class LRUCache(private val capacity: Int) {

    private val map = HashMap<Int, Node>()
    private val cache = DoubleList()

    fun get(key: Int): Int {
        if (!map.contains(key)) {
            return -1
        }

        makeRecently(key)
        return map[key]!!.val
    }

    fun put(key: Int, value: Int) {
        // 考慮 key 是否已存在
        if (map.contains(key)) {
            // 先刪掉
            deleteKey(key)
            // 加到最新使用
            addRecently(key, value)
            return
        }

        // 如果 cache 已滿
        if (capacity == cache.size) {
            // 刪掉許久未用的
            removeLeastRecently()
        }

        // 加到最新使用
        addRecently(key, value)
    }

    private fun addRecently(key: Int, value: Int) {
        val node = Node(key, value)
        // 加入 cache
        cache.addLast(node)
        // 加入 map
        map[key] = node
    }

    private fun deleteKey(int key) {
        val node = map.getOrDefault(key, null) ?: return
        // 移出 cache
        cache.remove(node)
        // 移出 map
        map.remove(key)

    }

    private fun makeRecently(key: Int) {
        val node = map.getOrDefault(key, null) ?: return
        // 刪掉
        cache.remove(node)
        // 加回來
        cache.addLast(node)
    }

    private fun removeLeastRecently() {
        val first = cache.removeFirst() ?: return
        map.remove(first.key)
    }
}

class Node(val key: Int, val value: Int) {
    var next: Node? = null
    var prex: Node? = null
}

class DoubleList {
    private var head: Node = Node(0, 0)
    private var tail: Node = Node(0, 0)

    // 鏈結串列大小
    var size: Int = 0 
        private set

    init {
        head.next = tail
        tail.prev = head
    }

    // 在 鏈結串列尾部增加節點 node，時間複雜度 O(1)
    fun addLast(node: Node) {
        node.prev = tail.prev
        node.next = tail
        tail.prev?.next = node
        tail.prev = node
        size++
    }

    // 移除鏈結串列中節點 node (node 一定存在)
    private fun remove(node: Node) {
        node.prev?.next = node.next
        node.next?.prev = node.prev
        size--
    }

    // 移除鏈結串列中第一個節點 node
    fun removeFirst(): Node? {
        // 如果鏈結串列為空
        if (head.next == tail) { 
            return null
        }

        val first = head.next ?: return null
        remove(first)
        return first
    }
}
```

**使用LinkedHashMap**
```kotlin
class LRUCache(private val capacity: Int) {
    private val cache = LinkedHashMap<Int, Int>()

    fun get(key: Int): Int {
        if (!cache.containsKey(key)) {
            return -1
        }

        makeRecently(key)
        return cache[key]!!
    }

    fun put(key: Int, value: Int) {
        if (cache.contains(key)) {
            cache.put(key, value)
            makeRecently(key)
            return 
        }

        if (cache.size >= capacity) {
            val oldestKey = cache.keySet().first()
            cache.remove(oldestKey)
        }
        cache.put(key, value)
    }

    private fun makeRecently(key: Int) {
        val value = cache.getOrDefault(key, null) ?: return
        cache.remove(key)
        cache.put(key, value)
    }
}
```