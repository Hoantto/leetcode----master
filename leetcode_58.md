### 字符串处理
这种题差不多得嘞，就用`python`字符串处理API就完事。

**代码**
*python version*
```
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
     return len(s.strip().split(" ")[-1])
```

**参考**
- [Python String strip() Method](https://www.w3schools.com/python/ref_string_strip.asp)