### 正则匹配
主要是熟悉正则表达式的语法。

**代码**
*python version*
```
class Solution:
    def isNumber(self, s: str) -> bool:
        regex = "[+-]?((\d+)|(\d+\.\d*)|(\d*\.\d+))([eE][+-]?\d+)?"
        if re.fullmatch(regex, s) is not None:
            return True
        return False
```