### 模拟
很无聊的一道题。
对于每一行，首先需要确定最多可以放置多少个单词，这样可以得到改行的空格个数，从而确定改行单词之间的空格个数。
根据题目中填充空格的细节，可以分为以下三种情况讨论：
- 当前行是最后一行：单词左对齐，且单词之间只有一个空格，在行末填充剩余的空格
- 当前行不是最后一行，且只有一个单词：该单词左对齐，在行末填充空格
- 当前行不是最后一行，且不止一个单词：设当前行单词数为$numWords$，空格数为$numSpaces$，我们需要将空格均匀分配在单词之间，则单词之间至少有
$$avgSpaces=\lfloor\frac{numSpaces}{numWords-1}\rfloor$$
个空格，对于多出来的
$$extraSpaces=numSpaces\ mod(numWords-1)$$
个空格，应该在前$extraSpaces$个单词之间。因此，前$extraSpaces$个单词之间填充$avgSpaces+1$个空格，其余单词之间填充$avgSpaces$个空格。

**代码**
*python version*
```
# blank 返回长度为 n 的由空格组成的字符串
def blank(n: int) -> str:
    return ' ' * n

class Solution:
    def fullJustify(self, words: List[str], maxWidth: int) -> List[str]:
        ans = []
        right, n = 0, len(words)
        while True:
            left = right  # 当前行的第一个单词在 words 的位置
            sumLen = 0  # 统计这一行单词长度之和
            # 循环确定当前行可以放多少单词，注意单词之间应至少有一个空格
            while right < n and sumLen + len(words[right]) + right - left <= maxWidth:
                sumLen += len(words[right])
                right += 1

            # 当前行是最后一行：单词左对齐，且单词之间应只有一个空格，在行末填充剩余空格
            if right == n:
                s = " ".join(words[left:])
                ans.append(s + blank(maxWidth - len(s)))
                break

            numWords = right - left
            numSpaces = maxWidth - sumLen

            # 当前行只有一个单词：该单词左对齐，在行末填充空格
            if numWords == 1:
                ans.append(words[left] + blank(numSpaces))
                continue

            # 当前行不只一个单词
            avgSpaces = numSpaces // (numWords - 1)
            extraSpaces = numSpaces % (numWords - 1)
            s1 = blank(avgSpaces + 1).join(words[left:left + extraSpaces + 1])  # 拼接额外加一个空格的单词
            s2 = blank(avgSpaces).join(words[left + extraSpaces + 1:right])  # 拼接其余单词
            ans.append(s1 + blank(avgSpaces) + s2)

        return ans

```