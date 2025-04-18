# MinHeap

## Implementation
```kotlin
class MinHeap(private var capacity: Int) {
    private var heap = IntArray(capacity) { Int.MIN_VALUE }
    private var size = 0

    private fun parent(pos: Int) = floor((pos - 1).toDouble() / 2).toInt()
    private fun left(pos: Int) = 2 * pos + 1
    private fun right(pos: Int) = 2 * pos + 2
    private fun isLeaf(pos: Int) = pos > parent(size - 1)
    private fun swap(i: Int, j: Int) {
        val temp = heap[i]
        heap[i] = heap[j]
        heap[j] = temp
    }

    fun insert(element: Int) {
        // do nothing if the heap is full
        if (size >= capacity) {
            return
        }

        // put element at the end of head
        heap[size] = element
        size++
        heapifyUp()
    }

    fun poll(): Int {
        if (size == 0) throw IllegalStateException()
        val poped = heap[0]
        heap[0] = heap[size - 1]
        size--
        // heapify down
        heapifyDown(0)
        return poped
    }

    private fun heapifyUp() {
        var i = size - 1

        // heapify up
        while (i > 0 && heap[i] < heap[parent(i)]) {
            swap(i, parent(i))
            i = parent(i)
        }
    }

    private fun heapifyDown(pos: Int) {
        if (!isLeaf(pos)) {
            // determine swapPos from left and right child
            val swapPos = if (right(pos) < size) {
                if (heap[left(pos)] < heap[right(pos)]) {
                    left(pos)
                } else {
                    right(pos)
                }
            } else {
                left(pos)
            }
            if (heap[pos] > heap[left(pos)] || heap[pos] > heap[right(pos)]) {
                swap(pos, swapPos)
                heapifyDown(swapPos)
            }
        }
    }
}
```