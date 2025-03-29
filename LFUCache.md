# LFUCache

```kotlin
class LFCCache(private val capacity) {
    // key to freq
    private val keyToFreq = HashMap<Int, Int>()

    // key to value
    private val keyToValue = HashMap<Int, Int>()

    // freq to keys
    private val freqToKeys = HashMap<Int, LinkedHashSet<Int>>()

    private var minFreq = 1

    fun get(key: Int) Int {
        // key doesn't exist, return -1 
        if (!keyToValue.containsKey(key)) {
            return -1 
        }

        increaseFreq(key)
        return keyToValue[key]!!
    }

    fun put(key: Int, value: Int) {
        // key exist, update value and increase freq
        if (keyToValue.containsKey(key)) {
            keyToValue[key] = value
            increaseFreq(key)
            return
        }

        // key doesn't exist, try to put key/value in
        if (keyToValue.size >= capacity) {
            removeKeyWithMinFreq()
        }

        // update KV
        keyToValue[key] = value

        // update KF
        keyToFreq[key] = 1

        // update FK
        freqToKeys.putIfAbsent(1, LinkedHashSet<Int>())
        freqToKeys[1]?.add(key)
        minFreq = 1
    }

    private fun increaseFreq(key: Int) {
        val originalFreq = keyToFreq[key] ?: return
        // Update KF
        keyToFreq[key] = originalFreq + 1

        // Update FK
        freqToKeys[originalFreq]?.remove(key)
        freqToKeys.putIfAbsent(originalFreq + 1, LinkedHashSet<Int>())
        freqToKeys[originalFreq + 1]?.add(key)

        // if the keys associated with originalFreq is empty
        // remove it and update minFreq is necessary
        if (freqToKeys[originalFreq].isNullOrEmpty) {
            freqToKeys.remove(originalFreq)
            if (originalFreq == minFreq) {
                minFreq++
            }
        }
    }

    private fun removeKeyWithMinFreq() {
        val minFreqKeys = freqToKeys[minFreq] ?: return
        val deleteKey = minFreqKeys.firstOrNull() ?: return

        // Update KV
        keyToValue.remove(deleteKey)

        // Update KF
        keyToFreq.remove(deleteKey)

        // Update FK
        minFreqKeys.remove(deleteKey)
        if (minFreqKeys.isEmpty()) {
            freqToKeys.remove(minFreq)
        }
    }
}