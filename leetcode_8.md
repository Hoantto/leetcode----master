### 正则表达式
[正则表达式学习](https://www.runoob.com/regexp/regexp-syntax.html)
[python'*'用法](https://treyhunner.com/2018/10/asterisks-in-python-what-they-are-and-how-to-use-them/)
[python re](https://docs.python.org/3/library/re.html)

**代码**
*python version*
```
class Solution:
    def myAtoi(self, s: str) -> int:
        return max(min(int(*re.findall('^[\+\-]?\d+', s.lstrip())), 2**31 - 1), -2**31)
```