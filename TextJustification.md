# 68. Text Justification

Given an array of strings `words` and a width `maxWidth`, format the text such that each line has exactly `maxWidth` characters and is fully (left and right) justified.

You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces `' '` when necessary so that each line has exactly `maxWidth` characters.

Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.

For the last line of text, it should be left-justified, and no extra space is inserted between words.

Note:

- A word is defined as a character sequence consisting of non-space characters only.
- Each word's length is guaranteed to be greater than `0` and not exceed `maxWidth`.
- The input array `words` contains at least one word.
 
**Example 1:**
```
Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
Output:
[
   "This    is    an",
   "example  of text",
   "justification.  "
]
```

**Example 2:**
```
Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
Output:
[
  "What   must   be",
  "acknowledgment  ",
  "shall be        "
]
Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
Note that the second line is also left-justified because it contains only one word.
```

**Example 3:**
```
Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
Output:
[
  "Science  is  what we",
  "understand      well",
  "enough to explain to",
  "a  computer.  Art is",
  "everything  else  we",
  "do                  "
]
``` 

Constraints:

1 <= words.length <= 300
1 <= words[i].length <= 20
words[i] consists of only English letters and symbols.
1 <= maxWidth <= 100
words[i].length <= maxWidth

## Solution

```kotlin
class Solution {
    fun fullJustify(words: Array<String>, maxWidth: Int): List<String> {
        val result = mutableListOf<String>()
        val line = mutableListOf<String>()
        var lineLength = 0
        var index = 0
        val n = words.size

        while (index < n) {
            val word = words[index]
            if (lineLength + line.size + word.length > maxWidth) {
                addFullyJustifiedLine(line, result, lineLength, maxWidth)
                lineLength = 0
                line.clear()
            }
            line += word
            lineLength += word.length
            index++
        }

        if (line.isNotEmpty()) {
            addLeftJustifiedLine(line, result, lineLength, maxWidth)
        }

        return result
    }

    private fun addFullyJustifiedLine(
        line: List<String>,
        result: MutableList<String>,
        length: Int,
        maxWidth: Int
    ) {
        val spacesCount = (maxWidth - length) / max(1, (line.size - 1))
        var remainderCount = (maxWidth - length) % max(1, (line.size - 1))
        val spaces = generateSpaces(spacesCount)

        val sb = StringBuilder()
        if (line.size > 1) {
            for (i in 0..<line.size - 1) {
                sb.append(line[i]).append(spaces)
                if (remainderCount > 0) {
                    sb.append(" ")
                    remainderCount--
                }
            }
            sb.append(line.last())
        } else {
            sb.append(line[0]).append(spaces)
        }
        result.add(sb.toString())
    }

    private fun addLeftJustifiedLine(
        line: List<String>,
        result: MutableList<String>,
        length: Int,
        maxWidth: Int
    ) {
        if (line.isEmpty()) return
        val remainingSpace = maxWidth - length - (line.size - 1)
        val sb = StringBuilder()
        sb.append(line.joinToString(" ")).append(generateSpaces(remainingSpace))
        result.add(sb.toString())
    }

    private fun generateSpaces(spaces: Int): String {
        var num = spaces
        val spacesBuilder = StringBuilder()
        while (num > 0) {
            spacesBuilder.append(" ")
            num--
        }
        return spacesBuilder.toString()
    }
}
```