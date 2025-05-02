# 418. Sentence Screen Fitting

Given a <code>rows x cols</code> screen and a <code>sentence</code> represented as a list of strings, return <em>the number of&nbsp;times the given sentence can be fitted on the screen</em>.

The order of words in the sentence must remain unchanged, and a word cannot be split into two lines. A single space must separate two consecutive words in a line.


**Example 1:**

<pre>
<strong>Input:</strong> sentence = [&quot;hello&quot;,&quot;world&quot;], rows = 2, cols = 8
<strong>Output:</strong> 1
<strong>Explanation:</strong>
hello---
world---
The character &#39;-&#39; signifies an empty space on the screen.
</pre>

**Example 2:**

<pre>
<strong>Input:</strong> sentence = [&quot;a&quot;, &quot;bcd&quot;, &quot;e&quot;], rows = 3, cols = 6
<strong>Output:</strong> 2
<strong>Explanation:</strong>
a-bcd- 
e-a---
bcd-e-
The character &#39;-&#39; signifies an empty space on the screen.
</pre>

**Example 3:**

<pre>
<strong>Input:</strong> sentence = [&quot;i&quot;,&quot;had&quot;,&quot;apple&quot;,&quot;pie&quot;], rows = 4, cols = 5
<strong>Output:</strong> 1
<strong>Explanation:</strong>
i-had
apple
pie-i
had--
The character &#39;-&#39; signifies an empty space on the screen.
</pre>

**Constraints:**

<ul>
	<li><code>1 &lt;= sentence.length &lt;= 100</code></li>
	<li><code>1 &lt;= sentence[i].length &lt;= 10</code></li>
	<li><code>sentence[i]</code> consists of lowercase English letters.</li>
	<li><code>1 &lt;= rows, cols &lt;= 2 * 10<sup>4</sup></code></li>
</ul>

## Solution
```kotlin
class Solution {
	fun wordsTyping(sentence: Array<String>, rows: Int, cols: Int): Int {
		// 將整個句子合併為一個字串，單字之間用空格隔開，並在結尾加上一個空格。
        // 例如：["hello", "world"] → "hello world "
        val combined = sentence.joinToString(" ") + " "
        val n = combined.length // combined 字串的總長度
        var i = 0 // 用來記錄目前走過的字元數（跨越多個 row 時累加）

		// 對每一列進行模擬填字
        repeat(rows) {
			// 假設我們可以在本列填滿 cols 個字元
            i += cols

			// 如果剛好落在空格上，代表該行剛好填完一個單字，直接跳過空格到下一個字的第一個字元
            if (combined[i % n] == ' ') {
                i++  // move to the start of next word
            } else {
				// 否則，我們正在中斷一個單字，需要往前回溯，該中斷字的第一個字元
                while (i > 0 && combined[(i - 1) % n] != ' ') {
                    i--  
                }
            }
        }

		// 回傳能夠完整重複 combined 字串的次數（因為每次重複都是完整的一句 sentence）
        return i / n
    }
}
```